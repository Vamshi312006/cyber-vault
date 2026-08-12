---
id: "mod-nix-syscall-vdso"
title: "Linux System Call Interface, vDSO & vsyscall"
domain: "Domain-01"
branch: "linux-kernel"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux System Call Interface, vDSO & vsyscall

## 1. Overview & Purpose
The Linux system call interface provides the execution bridge allowing user-space applications to request OS kernel services (file I/O, process creation, network sockets).

This module details the assembly-level `SYSCALL` instruction mechanics in x86_64 Linux, system call dispatch tables (`sys_call_table`), virtual Dynamic Shared Objects (`vDSO`), legacy `vsyscall` memory mappings, and `seccomp-bpf` system call filtering. Mastery of this interface is essential for binary exploitation (ROP/syscall chains), container sandboxing, and eBPF system call auditing.

---

## 2. Architecture & Call Pipeline

```mermaid
graph TD
    subgraph Ring 3 (User Space)
        APP["User Application / GLIBC Wrapper<br/>(e.g., write / execve / gettimeofday)"]
        VDSO["vDSO Shared Library Area<br/>[vdso] (Fast user-space gettimeofday)"]

        APP -->|Standard Syscall| SYSCALL_INST
        APP -.->|Fast Syscall| VDSO
    end

    subgraph Hardware Boundary Transition
        SYSCALL_INST["CPU SYSCALL Instruction<br/>(RAX = Syscall Number; RDI, RSI, RDX, R10, R8, R9)"]
    end

    subgraph Ring 0 (Kernel Space)
        ENTRY["entry_SYSCALL_64 (arch/x86/entry/entry_64.S)"]
        SECCOMP["Seccomp BPF Filter Check"]
        TABLE["sys_call_table [RAX]"]
        HANDLER["sys_execve / sys_write (Kernel Function)"]

        ENTRY --> SECCOMP
        SECCOMP --> TABLE
        TABLE --> HANDLER
    end

    SYSCALL_INST --> ENTRY
```

---

## 3. Detailed Mechanics & Execution Pipeline

### 3.1 Linux x86_64 System Call Convention
In 64-bit x86 Linux, system calls use registers rather than stack parameters:
- **Syscall Number**: Loaded into register `RAX` (e.g., `0` = `read`, `1` = `write`, `2` = `open`, `59` = `execve`).
- **Arguments**: `RDI` (1st), `RSI` (2nd), `RDX` (3rd), `R10` (4th), `R8` (5th), `R9` (6th).
- **Return Value**: Returned in `RAX` (negative values between `-1` and `-4095` represent `-errno`).

#### Assembly Syscall Example (x86_64 Assembly):
```assembly
mov rax, 59             ; sys_execve
lea rdi, [rel filename] ; Arg 1: const char *filename ("/bin/sh")
mov rsi, 0              ; Arg 2: char *const argv[] (NULL)
mov rdx, 0              ; Arg 3: char *const envp[] (NULL)
syscall                 ; Transition to Ring 0
```

---

### 3.2 `vDSO` (Virtual Dynamic Shared Object) & `vsyscall`
Certain frequent system calls (such as `gettimeofday()`, `clock_gettime()`, `time()`) cause excessive CPU overhead if every invocation requires a full Ring 3 -> Ring 0 context switch.

1. **`vsyscall` (Legacy)**: Fixed memory page at `0xffffffffff600000`. Deprecated due to static memory location enabling ROP exploitation.
2. **`vDSO` (Modern)**: Small shared library automatically mapped by the kernel into every user process address space (`[vdso]`). It exports kernel-provided code that reads kernel time data directly from user-space memory, completely avoiding Ring 0 hardware transitions.

---

### 3.3 Seccomp BPF (Secure Computing Mode)
Seccomp BPF allows a process to attach a Berkeley Packet Filter (BPF) program to its system call dispatcher:
- **`SECCOMP_MODE_STRICT`**: Restricts system calls exclusively to `read()`, `write()`, `exit()`, and `sigreturn()`.
- **`SECCOMP_MODE_FILTER`**: Evaluates custom BPF rules on incoming system calls based on syscall number (`RAX`) and argument values.
- **Return Actions**: `SECCOMP_RET_ALLOW`, `SECCOMP_RET_ERRNO`, `SECCOMP_RET_KILL_PROCESS`, `SECCOMP_RET_TRAP`.

---

## 4. Security Implications & Sandbox Boundaries

- **Seccomp Sandboxing**: Used extensively in Docker, Kubernetes, Chrome, and systemd to restrict available system call attack surfaces. For example, blocking `unshare` or `clone` prevents container escape attempts.
- **Syscall Hooking in Modern Kernels**: Direct modification of `sys_call_table` is prevented by kernel read-only memory protections (`CR0.WP` bit). Modern kernel tracing relies on eBPF or ftrace.

---

## 5. Attack Vectors & Exploitation Primitives

1. **ROP Chains using `syscall` Instructions**:
   In binary exploitation (Stack Overflows), attackers construct Return-Oriented Programming (ROP) chains to control `RAX`, `RDI`, `RSI`, `RDX`, and jump to a `syscall; ret` gadget to invoke `execve("/bin/sh")`.
2. **Seccomp BPF Architecture Bypass (x86 vs x86_64)**:
   If a seccomp filter checks system call numbers without validating the architecture field (`sys_audit_data.arch`), an attacker can execute 32-bit `int 0x80` syscalls to bypass 64-bit `syscall` filters.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Auditd System Call Telemetry**: Logs full syscall execution details including syscall number, arguments, and process context.
- **eBPF Tracepoints**: `raw_syscalls/sys_enter`, `raw_syscalls/sys_exit`.

### Inspecting Process Seccomp Filters:
```bash
# View Seccomp Status for PID
grep Seccomp /proc/[pid]/status
# Seccomp: 2  (0 = Disabled, 1 = Strict, 2 = Filter)
```

---

## 7. Engineering & Hands-On Implementation

### Tracing System Calls via `strace`:
```bash
# Trace execve and openat system calls
strace -e trace=execve,openat /bin/ls /tmp
```

### Dumping vDSO Memory Mapping:
```bash
# Find vDSO address range
cat /proc/self/maps | grep vdso
# 7fff8d5fb000-7fff8d5fd000 r-xp 00000000 00:00 0   [vdso]
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Application crashes with `Bad system call` (`SIGSYS`). | Process triggered a syscall blocked by an active Seccomp filter. | Run `strace` or inspect `dmesg` to identify the blocked syscall number. |
| `strace` fails to trace process. | `ptrace` system call restricted by YAMA ptrace scope (`/proc/sys/kernel/yama/ptrace_scope`). | Set `ptrace_scope` to `0` or execute `strace` as root. |

---

## 9. References
- Michael Kerrisk, *The Linux Programming Interface*.
- Linux Kernel Documentation: *Seccomp BPF & vDSO System Calls*.
