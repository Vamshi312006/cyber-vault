---
id: "mod-win-syscall-ntdll"
title: "Windows Native API & System Call Dispatching"
domain: "Domain-01"
branch: "windows-internals"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Native API & System Call Dispatching

## 1. Overview & Purpose
The Windows system call interface is the boundary separating unprivileged user-mode execution (Ring 3) from privileged kernel-mode operations (Ring 0).

This module details the `Kernel32.dll` -> `NTDLL.DLL` -> Kernel transition pipeline, the System Service Descriptor Table (SSDT), System Service Numbers (SSNs), assembly-level `SYSCALL` instruction mechanics, and direct versus indirect system call evasion tradecraft. Master of this interface is essential for detection engineering, EDR hook bypass analysis, and kernel telemetry collection.

---

## 2. Architecture & Call Pipeline

```mermaid
graph TD
    subgraph Ring 3 (User Space)
        APP["Application (e.g., malware.exe / cmd.exe)"]
        K32["Kernel32.dll / KernelBase.dll<br/>(e.g., CreateFileW / VirtualAllocEx)"]
        NTDLL["NTDLL.DLL (Native API)<br/>(e.g., NtCreateFile / NtAllocateVirtualMemory)"]

        APP --> K32
        K32 --> NTDLL
        APP -.->|Direct / Indirect Syscall| NTDLL
    end

    subgraph Hardware Boundary Transition
        SYSCALL_INST["CPU SYSCALL Instruction<br/>(Reads MSR 0xC0000082 - IA32_LSTAR)"]
    end

    subgraph Ring 0 (Kernel Space)
        ENTRY["KiSystemCall64 / KiSystemCall64Shadow<br/>(Kernel Entry Point)"]
        SSDT["System Service Descriptor Table (SSDT)<br/>(KeServiceDescriptorTable)"]
        KERN_FUNC["nt!NtAllocateVirtualMemory<br/>(Kernel Implementation)"]

        ENTRY --> SSDT
        SSDT --> KERN_FUNC
    end

    NTDLL --> SYSCALL_INST
    SYSCALL_INST --> ENTRY
```

---

## 3. Detailed Mechanics & Execution Pipeline

### 3.1 The Win32 API to Native API Abstraction
High-level Windows APIs (`CreateFileW`, `VirtualAllocEx`, `OpenProcess`) in `Kernel32.dll` or `KernelBase.dll` do not contain system call instructions themselves. They act as wrappers that validate arguments before invoking undocumented Native API functions (`NtCreateFile`, `NtAllocateVirtualMemory`, `NtOpenProcess`) exported by `NTDLL.DLL`.

#### Assembly Stub in `NTDLL.DLL` (x64 Windows 11):
```assembly
; NtAllocateVirtualMemory in NTDLL.DLL
mov r10, rcx          ; Copy 1st argument pointer to r10 (Win x64 Calling Convention)
mov eax, 018h         ; Load System Service Number (SSN) for NtAllocateVirtualMemory into EAX
test byte ptr [SharedUserData+0x308], 1 ; Check if syscall requires special mitigation
jnz fallback
syscall               ; Transition CPU to Ring 0 (IA32_LSTAR MSR)
ret
fallback:
int 2Eh               ; Legacy interrupt fallback
ret
```

---

### 3.2 Hardware Ring Transition (`SYSCALL` & `IA32_LSTAR`)
1. **Executing `SYSCALL`**: When the CPU executes `syscall` in Ring 3:
   - User RIP is saved to `RCX`.
   - RFLAGS register is saved to `R11`.
   - CPU Ring is switched from Ring 3 to Ring 0.
   - RIP is set to the value stored in Model-Specific Register `IA32_LSTAR` (`0xC0000082`), which holds `nt!KiSystemCall64`.
2. **Kernel Entry (`KiSystemCall64`)**:
   - Swaps stack pointer (`GS` register) from User Stack to Kernel Stack.
   - Saves user context registers onto the kernel trap frame (`_KTRAP_FRAME`).
   - Extracts System Service Number (SSN) from `EAX`.

---

### 3.3 System Service Descriptor Table (SSDT) Resolution
The kernel uses the SSN in `EAX` to index into the `KeServiceDescriptorTable`:

$$\text{Kernel Function Address} = \text{KiServiceTable} + (\text{KiServiceTable}[\text{SSN}] \gg 4)$$

The kernel verifies argument count, extracts arguments from user stack or registers (`r10`, `rdx`, `r8`, `r9`), executes the kernel function (e.g., `nt!NtAllocateVirtualMemory`), restores `_KTRAP_FRAME`, and returns to user space via `SYSRET`.

---

## 4. Security Implications & EDR User-Mode Hooking

Endpoint Detection and Response (EDR) agents monitor application behavior in user space by installing **Inline API Hooks** inside `NTDLL.DLL` memory stubs for sensitive Native APIs (`NtOpenProcess`, `NtAllocateVirtualMemory`, `NtWriteVirtualMemory`, `NtCreateThreadEx`).

```text
Un-hooked NTDLL Assembly Stub:
mov r10, rcx
mov eax, 18h
syscall
ret

EDR Hooked NTDLL Assembly Stub:
jmp <EDR_Sensor_DLL.dll!HookRoutine>  ; 5-byte JMP overwrites initial bytes
mov eax, 18h
syscall
ret
```

When an application calls `NtAllocateVirtualMemory`, execution jumps to the EDR DLL, which inspects parameters for malicious patterns before returning execution to `NTDLL.DLL`.

---

## 5. Attack Vectors & Evasion Tradecraft

1. **Direct System Calls**:
   Bypassing user-mode EDR hooks by embedding raw `mov eax, SSN; syscall` assembly instructions directly inside the malware binary, completely omitting `NTDLL.DLL` function calls.
2. **Indirect System Calls (e.g., HellsGate / HaloGate / RecycledGate)**:
   Dynamically resolving the SSN from `NTDLL.DLL` in memory, but executing the `syscall; ret` instruction *inside* `NTDLL.DLL` memory space. This satisfies EDR telemetry checks that verify RIP originates from an official module (`NTDLL.DLL`) rather than unbacked malware memory.
3. **Un-hooking `NTDLL.DLL` (FreshyCalls / Perun's Fart)**:
   Reading a clean copy of `NTDLL.DLL` from disk (or `\KnownDlls\ntdll.dll`) and overwriting the in-memory hooked `.text` section of `NTDLL.DLL` to restore original assembly stubs.

---

## 6. Defense & Telemetry Verification

### Detection Strategies for Direct/Indirect Syscalls:
- **User-mode hooking is insufficient**: Direct/indirect syscalls bypass Ring 3 hooks.
- **Kernel-Level ETW Tracing**: `Microsoft-Windows-Threat-Intelligence` (`ETW-TI`) emits kernel events for process creation, remote memory allocation, and thread creation directly from `nt!NtAllocateVirtualMemory` and `nt!NtOpenProcess`.
- **Call Stack Telemetry**: Inspecting stack frames in `ETW-TI` events to verify if caller RIP originates from expected DLL locations.

### ETW-TI Telemetry Event Fields:
- `CallingProcessId`: PID executing the call.
- `TargetProcessId`: Target PID being modified.
- `DesiredAccess`: Requested access mask.
- `CallStack`: Stack frame backtrace (reveals unbacked memory addresses in Direct Syscalls).

---

## 7. Engineering & Hands-On Implementation

### Inspecting SSDT and `KiSystemCall64` in WinDbg:
```text
kd> rdmsr 0xc0000082
msr[c0000082] = fffff801`2a412040  <- KiSystemCall64 Entry Point

kd> u fffff801`2a412040 L5
nt!KiSystemCall64:
fffff801`2a412040 0f01f8          swapgs
fffff801`2a412043 6548892425100000 mov   gs:[10h], rsp
fffff801`2a41204b 65488b2425a80000 mov   rsp, gs:[a8h]

kd> dps nt!KiServiceTable L5
fffff801`2a812000  00412304
fffff801`2a812004  01824508
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Direct syscall execution fails with `0xC0000005`. | Invalid SSN used (SSNs change across Windows build numbers). | Dynamically resolve SSNs at runtime by sorting `NTDLL.DLL` exports by address (HaloGate method). |
| EDR flags process creation during Direct Syscall. | `ETW-TI` kernel provider detected call stack anomaly (RIP originated outside signed DLL). | Use Indirect Syscalls to jump to legitimate `syscall; ret` instructions inside `NTDLL.DLL`. |

---

## 9. References
- Pavel Yosifovich, *Windows Kernel Programming (2nd Edition)*.
- Outflank: *Direct Syscalls vs Indirect Syscalls in Threat Emulation*.
