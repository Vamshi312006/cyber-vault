---
id: "mod-nix-task-struct"
title: "Linux Process Subsystem & Task Struct Internals"
domain: "Domain-01"
branch: "linux-kernel"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux Process Subsystem & Task Struct Internals

## 1. Overview & Purpose
In the Linux kernel, every process and thread is represented as a execution context defined by the `task_struct` structure. Linux does not distinguish between threads and processes at the kernel level; both are `task_struct` instances created via `clone()`.

This module covers the internal layout of `task_struct`, process lifecycle calls (`fork`, `vfork`, `clone`, `execve`), credentials management (`struct cred`), UID/GID representation, namespace pointers, and kernel privilege escalation primitives. This knowledge is essential for container security analysis, eBPF probe development, and kernel exploitation.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Linux Kernel Space (Ring 0)
        TS["task_struct (Process Control Block)<br/>├── pid_t pid / tgid (PID & Thread Group ID)<br/>├── volatile long state (TASK_RUNNING, TASK_INTERRUPTIBLE)<br/>├── struct mm_struct *mm (Virtual Memory Space)<br/>├── struct files_struct *files (Open File Descriptors)<br/>├── struct nsproxy *nsproxy (Linux Namespaces)<br/>└── const struct cred *cred (Security Credentials)"]

        CRED["struct cred (Security Context)<br/>├── uid, gid (Real UID/GID)<br/>├── euid, egid (Effective UID/GID)<br/>├── suid, sgid (Saved UID/GID)<br/>├── fsuid, fsgid (FileSystem UID/GID)<br/>└── kernel_cap_t cap_inheritable, cap_permitted, cap_effective"]

        MM["mm_struct (Memory Descriptor)<br/>├── pgd_t *pgd (Page Global Directory)<br/>└── struct vm_area_struct *mmap (VMA LinkedList)"]

        TS --> CRED
        TS --> MM
    end

    subgraph User Space (Ring 3)
        PROC["User Process / Container Workload"]
    end

    TS -.->|System Call Boundary| PROC
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 The `task_struct` Layout (`include/linux/sched.h`)
Allocated via the SLAB/SLUB allocator, `task_struct` is the central node of Linux process management.

#### Key `task_struct` Fields:
- `pid_t pid`: Process ID (Thread ID in POSIX terms).
- `pid_t tgid`: Thread Group ID (Process ID in user-space terms; equal to `pid` for single-threaded processes).
- `struct list_head tasks`: Doubly linked list connecting all active `task_struct` nodes in system memory (`init_task`).
- `const struct cred __rcu *cred`: Pointer to process security credentials (UID, GID, Capabilities).
- `struct nsproxy *nsproxy`: Pointer to container namespace proxy structure (Mount, PID, Net, IPC, UTS, User).

---

### 3.2 Security Credentials (`struct cred`)
Linux access control decisions rely on the `struct cred` structure (`include/linux/cred.h`):

```c
struct cred {
    atomic_t    usage;
    kuid_t      uid;         /* Real User ID */
    kgid_t      gid;         /* Real Group ID */
    kuid_t      euid;        /* Effective User ID (Used for permission checks) */
    kgid_t      egid;        /* Effective Group ID */
    kuid_t      suid;        /* Saved User ID */
    kgid_t      sgid;        /* Saved Group ID */
    kuid_t      fsuid;       /* Filesystem User ID */
    kgid_t      fsgid;       /* Filesystem Group ID */
    kernel_cap_t cap_inheritable; /* Inheritable POSIX Capabilities */
    kernel_cap_t cap_permitted;   /* Permitted POSIX Capabilities */
    kernel_cap_t cap_effective;   /* Effective POSIX Capabilities */
};
```

#### Privilege Escalation Primitive:
When `euid == 0` (Root), permission checks evaluate to true. In Linux kernel exploits (e.g., Dirty COW, eBPF out-of-bounds write), the classic post-exploitation payload invokes:
`commit_creds(prepare_kernel_cred(0));`
This allocates a new `struct cred` filled with zeros (`uid=0`, `euid=0`, `cap_effective=ALL`) and updates the current task's `cred` pointer.

---

### 3.3 Process Creation Lifecycle (`clone`, `fork`, `execve`)
1. **`fork()`**: Invokes `sys_clone()` with `SIGCHLD` flags. Duplicates parent `task_struct`, creating copy-on-write (COW) memory pages.
2. **`clone()`**: Core process creation primitive. Accepts flags (`CLONE_VM`, `CLONE_FS`, `CLONE_FILES`, `CLONE_NEWPID`, `CLONE_NEWNS`) to determine whether resources are shared (creating threads) or isolated (creating container namespaces).
3. **`execve()`**: Replaces current process memory layout with a new ELF binary, resets signal handlers, and executes the entry point.

---

## 4. Security Implications & Boundary Controls

- **Capabilities Framework**: POSIX capabilities break down monolithic root privileges into 41 fine-grained rights (`CAP_SYS_ADMIN`, `CAP_NET_ADMIN`, `CAP_SYS_PTRACE`, `CAP_BPF`).
- **User Namespaces**: Allows a process to have `uid=0` (Root) inside its user namespace while remaining an unprivileged user (`uid=1000`) on the host system.

---

## 5. Attack Vectors & Exploitation Primitives

1. **Kernel Credential Overwrite (`commit_creds`)**:
   Leveraging a kernel memory corruption flaw (use-after-free, out-of-bounds write) to locate the current process `task_struct` and overwrite its `cred` pointer with root credentials.
2. **SUID Binary Abuse**:
   Executing binaries marked with SUID bit (`-rwsr-xr-x`). When `execve()` executes an SUID root binary, `euid` is elevated to `0` (Root), creating exploitation vectors via path traversal or environment manipulation (`LD_PRELOAD`).
3. **Container Escape via `CAP_SYS_ADMIN`**:
   If a container possesses `CAP_SYS_ADMIN`, it can execute `mount()` to attach the host root filesystem (`/dev/sda1`) inside the container directory tree, achieving full host takeover.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Auditd System Call Events**: `type=SYSCALL` (captures `execve`, `clone`, `setuid`, `capset`).
- **eBPF Tracepoints**: `sched/sched_process_exec`, `sys_enter_execve`.
- **Linux Security Modules (LSM)**: SELinux / AppArmor hooks on `bprm_check_security`.

### Auditd Telemetry Rule (Privilege Escalation Monitoring):
```text
-w /usr/bin/sudo -p x -k priv_esc
-w /usr/bin/su -p x -k priv_esc
-a always,exit -F arch=b64 -S setuid -F a0=0 -k setuid_root
```

---

## 7. Engineering & Hands-On Implementation

### Inspecting Process Credentials via `/proc`:
```bash
# View Process Credentials and Capabilities for PID 1234
cat /proc/1234/status | grep -E "Uid|Gid|Cap"

# Example Output:
# Uid:	1000	1000	1000	1000  (Real, Effective, Saved, Filesystem)
# Gid:	1000	1000	1000	1000
# CapInh:	0000000000000000
# CapPrm:	0000000000000000
# CapEff:	0000000000000000
```

### Decoding Capability Hex Bitmask:
```bash
capsh --decode=0000000000003ec0
# Decodes to: cap_chown, cap_dac_override, cap_fowner, cap_net_raw...
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Container process cannot bind to port 80. | Missing `CAP_NET_BIND_SERVICE` capability. | Add `--cap-add=NET_BIND_SERVICE` flag during container execution. |
| `setuid(0)` fails with `EPERM`. | Process lacks `CAP_SETUID` capability or `NO_NEW_PRIVS` bit is set. | Inspect `/proc/[pid]/status` for `NoNewPrivs: 1`. |

---

## 9. References
- Daniel P. Bovet, Marco Cesati, *Understanding the Linux Kernel (3rd Edition)*.
- Michael Kerrisk, *The Linux Programming Interface*.
