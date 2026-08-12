---
id: "mod-drv-ebpf-verifier"
title: "Linux eBPF Architecture, Verifier Mechanics & Runtime Safety"
domain: "Domain-01"
branch: "driver-security"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux eBPF Architecture, Verifier Mechanics & Runtime Safety

## 1. Overview & Purpose
Extended Berkeley Packet Filter (eBPF) is a revolutionary kernel technology that allows sandbox bytecodes to execute safely inside the Linux kernel without modifying kernel source code or loading traditional kernel modules.

This module details eBPF Virtual Machine registers, eBPF Map types (`BPF_MAP_TYPE_HASH`, `BPF_MAP_TYPE_RINGBUF`), the eBPF Static Verifier (DAG safety analysis, memory boundary checks), Kprobes / Tracepoints / XDP probes, and eBPF security telemetry. Understanding eBPF is critical for cloud-native observability, modern EDR design (Tetragon, Cilium), and kernel sandbox security.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph User Space (Ring 3)
        PROG_SRC["eBPF C Source Code (clang -target bpf)"]
        BYTECODE["eBPF Bytecode (.o ELF File)"]
        LOADER["eBPF Loader (bpf() Syscall)"]
        MAP_USER["User-Space App Reading Maps (libbpf)"]

        PROG_SRC --> BYTECODE
        BYTECODE --> LOADER
    end

    subgraph Linux Kernel (Ring 0 eBPF Execution Environment)
        VERIFIER["eBPF Verifier (DAG Control Flow & Memory Boundary Check)"]
        JIT["JIT Compiler (Translates eBPF Bytecode to Native x86_64 / ARM64 Machine Code)"]

        subgraph Probes & Hook Attachments
            KPROBE["Kprobes / Kretprobes (Kernel Functions)"]
            TRACEPOINT["Tracepoints (System Calls / Scheduler)"]
            XDP["XDP / TC (Network Packet Ingress / Egress)"]
        end

        MAP_KERN["eBPF Maps (Kernel Memory RingBuffer / Hash Arrays)"]

        VERIFIER -->|Passes Safety Checks| JIT
        JIT --> KPROBE
        JIT --> TRACEPOINT
        JIT --> XDP
        JIT <--> MAP_KERN
    end

    LOADER --> VERIFIER
    MAP_USER <--> MAP_KERN
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 eBPF Virtual Machine Architecture
The eBPF virtual machine consists of 11 64-bit registers, a 512-byte stack, and zero-copy map storage:
- **`R0`**: Return value of eBPF helper functions or program exit status.
- **`R1` - `R5`**: Function argument registers passed to kernel helper functions.
- **`R6` - `R9`**: Callee-saved registers preserved across helper calls.
- **`R10`**: Read-only frame pointer for the 512-byte stack space.

---

### 3.2 The eBPF Static Verifier
To prevent custom user code from crashing the kernel, every eBPF program submitted via `bpf(BPF_PROG_LOAD)` must pass the **eBPF Verifier**:
1. **DAG Control Flow Check**: Verifies that the program contains no unconstrained loops or unreachable instructions (guarantees program termination).
2. **Memory Boundary Validation**: Ensures no out-of-bounds stack or map access occurs.
3. **Pointer Type Tracking**: Tracks whether a register holds a scalar value, stack pointer, map pointer, or kernel object pointer, blocking invalid dereferences.

---

### 3.3 eBPF Map Types
eBPF programs cannot access arbitrary kernel memory directly. They share state with user-space using **eBPF Maps**:
- `BPF_MAP_TYPE_HASH` / `ARRAY`: Key-value storage.
- `BPF_MAP_TYPE_RINGBUF`: High-performance, lockless ring buffer for streaming security telemetry to user space.

---

## 4. Security Implications & Observability Advantage

- **No Kernel Recompilation**: Allows security agents (like Cilium Tetragon) to hook kernel functions dynamically without risks associated with unstable third-party kernel modules.
- **Root Requirement**: Loading eBPF programs requires `CAP_BPF` or `CAP_SYS_ADMIN` privileges.

---

## 5. Attack Vectors & Verifier Exploitation

1. **eBPF Verifier Out-of-Bounds Exploits**:
   Exploiting speculative register bounds tracking bugs inside the eBPF Verifier to trick the verifier into calculating an incorrect register range, allowing arbitrary read/write access to kernel memory.
2. **eBPF Rootkits**:
   Malicious actors with root access can attach eBPF probes to `sys_enter_execve` to stealthily hide files or processes from standard user-space utilities.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Auditd System Call Logs**: `bpf` system call monitoring (`BPF_PROG_LOAD`, `BPF_MAP_CREATE`).
- **bpftool Diagnostic Utility**: Inspects loaded eBPF programs and attached hooks across the host.

---

## 7. Engineering & Hands-On Implementation

### Inspecting Active eBPF Programs via `bpftool`:
```bash
# List all loaded eBPF programs on the system
sudo bpftool prog list

# Dump JIT-compiled assembly of program ID 42
sudo bpftool prog dump jited id 42

# List attached eBPF map objects
sudo bpftool map list
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| `bpf()` syscall returns `EPERM` (`13`). | Process lacks `CAP_BPF` / `CAP_SYS_ADMIN` capability or unprivileged eBPF is disabled (`unprivileged_bpf_disabled=1`). | Run as root or add `CAP_BPF` capability to executable. |
| Verifier error: `R1 offset is out of bounds`. | Array index calculation could exceed boundary in verifier analysis. | Add explicit bounds check before array lookup (`if (idx < MAX_SIZE)`). |

---

## 9. References
- Daniel Borkmann, Alexei Starovoitov, *eBPF Architecture & Verifier Design*.
- Cilium / Isovalent Documentation: *eBPF Security Observability with Tetragon*.
