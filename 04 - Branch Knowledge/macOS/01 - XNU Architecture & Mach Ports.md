---
id: "mod-mac-xnu-mach"
title: "macOS XNU Kernel Architecture & Mach Port IPC"
domain: "Domain-01"
branch: "macos-security"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# macOS XNU Kernel Architecture & Mach Port IPC

## 1. Overview & Purpose
The macOS and iOS operating systems run on the **XNU (X is Not Unix)** hybrid kernel architecture, combining the Mach microkernel foundation, BSD POSIX subsystem, and IOKit / DriverKit object-oriented driver frameworks.

This module details the XNU kernel architecture, Mach Tasks vs BSD Processes, Mach Messaging & Port IPC (`mach_msg`), Mach Port Right security (`MACH_PORT_RIGHT_RECEIVE`, `MACH_PORT_RIGHT_SEND`), and privilege escalation primitives targeting Mach IPC endpoints. Understanding XNU internals is essential for macOS threat research, Endpoint Security framework (ESF) telemetry analysis, and iOS/macOS kernel vulnerability auditing.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph XNU Hybrid Kernel Architecture (Ring 0)
        MACH["Mach 3.0 Layer<br/>(Threads, Tasks, Memory Management, IPC Ports)"]
        BSD["BSD Subsystem Layer<br/>(POSIX APIs, Process Credentials, File Systems, Networking)"]
        IOKIT["IOKit / DriverKit Framework<br/>(C++ Object-Oriented Device Drivers)"]

        MACH <--> BSD
        MACH <--> IOKit
    end

    subgraph Mach IPC Messaging Model
        TASK_A["Mach Task A (Sender)"]
        PORT["Mach Port Object (Kernel IPC Queue)"]
        TASK_B["Mach Task B (Receiver)"]

        TASK_A -->|mach_msg() with Send Right| PORT
        PORT -->|mach_msg() with Receive Right| TASK_B
    end
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 XNU Layer Responsibilities
1. **Mach Layer**: Provides low-level primitives: tasks (`task_t`), threads (`thread_t`), memory objects, and Inter-Process Communication (IPC) via Mach Ports.
2. **BSD Layer**: Wraps Mach primitives inside POSIX-compliant process models (`proc_t`), credentials (`ucred`), file descriptors, signal handling, and BSD sockets.
3. **IOKit / DriverKit**: C++ driver environment managing hardware access. Modern macOS moves drivers out of kernel space into user-space **DriverKit** daemons.

---

### 3.2 Mach Tasks vs BSD Processes
In XNU, every user process consists of two linked representations:
- **`proc_t` (BSD Process)**: Stores PID, UID/GID, signals, and file descriptors.
- **`task_t` (Mach Task)**: Stores virtual memory space and Mach Port capability rights table (`ipc_space`).

---

### 3.3 Mach Messaging & Port IPC
Mach Ports are kernel-protected capability endpoints used for process communication:
- **Mach Port Rights**:
  - `MACH_PORT_RIGHT_SEND`: Capability to send messages to the port queue.
  - `MACH_PORT_RIGHT_RECEIVE`: Capability to pull messages from the port queue (Held by exactly one task).
  - `MACH_PORT_RIGHT_SEND_ONCE`: Temporary one-time response right.
- **`mach_msg()` API**: The primary IPC system call used by user processes to send and receive structured messages containing data, file descriptors, or port rights.

---

## 4. Security Implications & Task Port Vulnerabilities

- **`task_for_pid()` API**: Obtains the target process's Mach Task Port (`task_t`). Holding a process's Task Port grants **complete control** over its virtual memory (`mach_vm_read`, `mach_vm_write`) and execution threads (`thread_create`), equivalent to `PROCESS_ALL_ACCESS` on Windows.
- **Task Port Entitlement Protections**: Modern macOS restricts `task_for_pid(self_pid, target_pid, &task)` using SIP (System Integrity Protection) and entitlement verification (`com.apple.system-task-ports`).

---

## 5. Attack Vectors & Exploitation Primitives

1. **Mach Port Name Spoofing & Race Conditions**:
   Exploiting IPC registration races during daemon initialization (e.g., `launchd` service registration) to substitute a rogue Mach Port, intercepting privileged IPC requests.
2. **Task Port Theft via Mach IPC Use-After-Free**:
   Exploiting dangling Mach Port references in XNU kernel memory to elevate privileges from unprivileged user to `kernel_task` (`task_t` port of the kernel itself), granting full kernel memory read/write.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **macOS Endpoint Security Framework (ESF)**: Emits `ES_EVENT_TYPE_NOTIFY_PROC_CHECK` and `ES_EVENT_TYPE_NOTIFY_GET_TASK`.
- **Unified Logging System**: `log stream --predicate 'subsystem == "com.apple.security"'`.

---

## 7. Engineering & Hands-On Implementation

### Inspecting Mach Ports using `lsmp` (macOS Command Line):
```bash
# List Mach Ports held by process PID 1234
lsmp -p 1234

# Display Task Ports and entitlements
codesign -d --entitlements :- /usr/libexec/tccd
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| `task_for_pid()` returns `KERN_FAILURE` (`5`). | SIP active or missing `com.apple.system-task-ports` entitlement. | Sign application with required entitlement or disable SIP in Recovery Mode. |
| Application crashes with `EXC_BAD_INSTRUCTION` (SIGILL). | Executed invalid Mach IPC call or signature entitlement validation failed. | Check crash log in `/Library/Logs/DiagnosticReports/`. |

---

## 9. References
- Jonathan Levin, *macOS and iOS Internals, Volume 1: User Mode & Volume 2: Kernel Mode*.
- Apple Developer Documentation: *XNU Kernel Programming & Mach IPC*.
