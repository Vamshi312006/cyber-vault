---
id: "mod-nix-namespaces-cgroups"
title: "Linux Namespaces, Control Groups (cgroups) & Container Isolation"
domain: "Domain-01"
branch: "linux-kernel"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux Namespaces, Control Groups (cgroups) & Container Isolation

## 1. Overview & Purpose
Containers (Docker, containerd, LXC) are not hardware virtual machines; they are standard Linux processes isolated by Linux kernel primitives: **Namespaces** (which isolate what a process can *see*) and **Control Groups / cgroups** (which limit what a process can *use*).

This module details the 8 Linux namespace types, `nsproxy` kernel structures, cgroups v1 versus v2 resource controllers, pivot_root mechanics, and container escape security primitives. Understanding these mechanisms is essential for cloud-native container security, K8s pod isolation, and container breakout investigations.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Linux Kernel (Ring 0)
        TASK["task_struct"]
        NSPROXY["struct nsproxy"]
        CGROUP_NODE["struct cgroup_subsys_state"]

        TASK --> NSPROXY
        TASK --> CGROUP_NODE
    end

    subgraph The 8 Linux Namespace Types (Visibility Boundaries)
        PID_NS["PID Namespace (Process Tree Isolation)"]
        MNT_NS["Mount Namespace (Filesystem Mount Table Isolation)"]
        NET_NS["Net Namespace (Network Interfaces, IP, Routing)"]
        IPC_NS["IPC Namespace (System V IPC, POSIX Message Queues)"]
        UTS_NS["UTS Namespace (Hostname & NIS Domain)"]
        USER_NS["User Namespace (UID/GID Mapping)"]
        CGROUP_NS["Cgroup Namespace (Virtual Cgroup Root View)"]
        TIME_NS["Time Namespace (Boottime & Monotonic Clock)"]

        NSPROXY --> PID_NS
        NSPROXY --> MNT_NS
        NSPROXY --> NET_NS
        NSPROXY --> IPC_NS
        NSPROXY --> UTS_NS
        NSPROXY --> USER_NS
        NSPROXY --> CGROUP_NS
        NSPROXY --> TIME_NS
    end

    subgraph Control Groups (Resource Resource Limits)
        MEM_CG["Memory Controller (RAM & Swap Limits)"]
        CPU_CG["CPU Controller (Shares & CFS Quota)"]
        IO_CG["I/O Controller (Disk Throttling)"]

        CGROUP_NODE --> MEM_CG
        CGROUP_NODE --> CPU_CG
        CGROUP_NODE --> IO_CG
    end
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 The 8 Linux Namespaces Explained
Every `task_struct` contains a pointer `nsproxy` (`include/linux/nsproxy.h`) holding pointers to namespace objects:

1. **PID (`CLONE_NEWPID`)**: Isolates process ID numbering. Inside a PID namespace, the container entry point receives `PID 1`, while having a standard PID (e.g., `PID 14205`) on the host.
2. **Mount (`CLONE_NEWNS`)**: Isolates file system mount points.
3. **Net (`CLONE_NEWNET`)**: Provides virtual network devices (`veth` pairs), private loopback, IP routing tables, and firewall rules.
4. **IPC (`CLONE_NEWIPC`)**: Isolates System V IPC objects and POSIX message queues.
5. **UTS (`CLONE_NEWUTS`)**: Isolates hostname and domain name.
6. **User (`CLONE_NEWUSER`)**: Maps UIDs and GIDs. Allows root inside container (`uid 0`) to map to unprivileged user (`uid 10001`) on host.
7. **Cgroup (`CLONE_NEWCGROUP`)**: Masks root cgroup directory pathing.
8. **Time (`CLONE_NEWTIME`)**: Isolates clock offsets (`CLOCK_MONOTONIC`, `CLOCK_BOOTTIME`).

---

### 3.2 Control Groups (cgroups v1 vs v2)
Control Groups restrict, log, and isolate resource usage (CPU, Memory, Disk I/O, Network):
- **cgroups v1**: Multi-hierarchy design where each resource controller (memory, cpu, blkio) operates an independent tree hierarchy under `/sys/fs/cgroup/`.
- **cgroups v2**: Unified single-hierarchy design (`/sys/fs/cgroup/cgroup.controllers`) resolving resource contention and inter-controller deadlock issues.

---

### 3.3 Root Filesystem Isolation (`pivot_root` vs `chroot`)
Container runtimes create a isolated root filesystem as follows:
1. Mount a target root directory (e.g., overlayfs rootfs) into a private Mount Namespace.
2. Execute `pivot_root(new_root, put_old)`.
3. Unmount `put_old` to ensure the host root filesystem (`/`) is completely unreachable from the container process tree.

---

## 4. Security Implications & Boundary Controls

- **Containers share the host kernel**: A kernel vulnerability (e.g., Dirty Pipe `CVE-2022-0847`) compromises the entire host regardless of container namespaces.
- **Privileged Containers (`--privileged`)**: Disables seccomp filters, grants all POSIX capabilities, and exposes host `/dev` devices, essentially rendering container boundaries void.

---

## 5. Attack Vectors & Container Escapes

1. **Host Socket Exposure (`/var/run/docker.sock`)**:
   If a container mounts the Docker socket, an attacker executes `docker run -v /:/host` to mount the host root filesystem into a new privileged container, gaining full host compromise.
2. **Exposed Cgroup Release Agent (`release_agent`)**:
   In cgroups v1, if a container has `CAP_SYS_ADMIN` and an exposed cgroup mount, writing a malicious script path to `cgroup.release_agent` triggers automatic host execution when a cgroup task exits.
3. **User Namespace UID Mapping Abuse**:
   If user namespaces are disabled, root inside the container (`uid 0`) is root on the host (`uid 0`). Any write permission to host files results in instant host privilege escalation.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Auditd Namespace Changes**: `setns`, `unshare` system call logs.
- **eBPF Process Monitoring**: Tracking container creation events via `cgroup` ID tags in Tracepoints.

### Inspecting Process Namespaces:
```bash
# View active namespace IDs for PID 1234
ls -l /proc/1234/ns/

# Output shows internal namespace inode IDs:
# lrwxrwxrwx 1 root root 0 net -> 'net:[4026532180]'
# lrwxrwxrwx 1 root root 0 pid -> 'pid:[4026532183]'
```

---

## 7. Engineering & Hands-On Implementation

### Spawning an Isolated Bash Shell using `unshare`:
```bash
# Create a new PID, Mount, and UTS namespace
sudo unshare --pid --mount --uts --fork /bin/bash

# Change hostname inside UTS namespace
hostname container-demo
hostname # Returns container-demo

# Check hostname on host in separate terminal (Returns host name)
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Container process killed unexpectedly (`Killed`). | Memory usage exceeded cgroup memory limit (`memory.max`). | Inspect `dmesg` or `/sys/fs/cgroup/memory.events` for `oom_kill` counter. |
| Cannot enter container namespace via `nsenter`. | Insufficient permissions or target process exited. | Execute `nsenter --target [PID] --mount --uts --ipc --net --pid`. |

---

## 9. References
- Michael Kerrisk, *The Linux Programming Interface (Namespaces Chapters)*.
- Docker Documentation: *Container Runtime Security & Namespaces*.
