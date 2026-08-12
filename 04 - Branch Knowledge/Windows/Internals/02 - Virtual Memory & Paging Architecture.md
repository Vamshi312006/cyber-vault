---
id: "mod-win-memory-paging"
title: "Windows Virtual Memory & Paging Architecture"
domain: "Domain-01"
branch: "windows-internals"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Virtual Memory & Paging Architecture

## 1. Overview & Purpose
Virtual Memory is the memory abstraction layer that presents each process with a flat, contiguous, private virtual address space while translating virtual addresses to physical RAM frames via CPU Memory Management Unit (MMU) page tables.

This module details the 64-bit Virtual Address Space (VAS) layout, 4-level x64 page table translation (`PML4 -> PDPT -> PD -> PT`), Virtual Address Descriptor (VAD) trees, Page Fault Handling (`#PF`), and memory allocation APIs. This knowledge is essential for analyzing process injection (Shellcode execution, Refective DLL Injection), memory protections, and kernel memory dumps.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph 64-bit Virtual Address Space (256 TB User / 256 TB Kernel)
        USER_SPACE["User-Mode VAS (0x00000000'00000000 to 0x00007FFF'FFFFFFFF)<br/>Private per process"]
        GAP["Non-Canonical Gap (Unusable addresses)"]
        KERNEL_SPACE["Kernel-Mode VAS (0xFFFF8000'00000000 to 0xFFFFFFFF'FFFFFFFF)<br/>Shared across all processes"]
    end

    subgraph MMU Address Translation (x64 4-Level Paging)
        CR3["CR3 Register (DirectoryTableBase)"] --> PML4["PML4 Table"]
        PML4 --> PDPT["Page Directory Pointer Table"]
        PDPT --> PD["Page Directory"]
        PD --> PT["Page Table"]
        PT --> PHYS["Physical RAM Frame (4KB / 2MB / 1GB Page)"]
    end
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 64-Bit Virtual Address Space (x64 Paging)
In 64-bit Windows, virtual addresses are 48-bit sign-extended canonical addresses:
- **User-Space**: `0x00000000'00000000` to `0x00007FFF'FFFFFFFF` (128 TB on x64; expandable up to 256 TB with 5-level paging).
- **Canonical Gap**: `0x00008000'00000000` to `0xFFFF7FFF'FFFFFFFF` (Addresses triggering CPU General Protection Fault `#GP`).
- **Kernel-Space**: `0xFFFF8000'00000000` to `0xFFFFFFFF'FFFFFFFF` (Shared system memory housing `NTOSKRNL`, drivers, non-paged pool).

---

### 3.2 MMU Page Translation Hardware Steps
For a 48-bit virtual address, the CPU MMU parses 9-bit bitfield offsets to traverse 4 page table levels:

```text
 47         39 38         30 29         21 20         12 11          0
┌─────────────┬─────────────┬─────────────┬─────────────┬─────────────┐
│ PML4 Index  │ PDPT Index  │  PD Index   │  PT Index   │ Page Offset │
│   (9 bits)  │   (9 bits)  │  (9 bits)   │  (9 bits)   │  (12 bits)  │
└─────────────┴─────────────┴─────────────┴─────────────┴─────────────┘
```

1. **`CR3` Register**: Contains physical base address of PML4 (Page Map Level 4) table for active process.
2. **PML4 Lookup**: Index (bits 39-47) yields PML4 Entry (PML4E) pointing to PDPT.
3. **PDPT Lookup**: Index (bits 30-38) yields PDPTE pointing to Page Directory.
4. **PD Lookup**: Index (bits 21-29) yields PDE pointing to Page Table (or Large 2MB Page frame).
5. **PT Lookup**: Index (bits 12-20) yields PTE containing physical RAM frame address.
6. **Offset**: Physical frame base address + 12-bit Page Offset (bits 0-11) = Physical RAM byte.

---

### 3.3 Virtual Address Descriptor (VAD) Tree
While page tables handle hardware MMU translation, the Windows Kernel tracks reserved and committed virtual memory regions using an AVL balanced binary tree called the **Virtual Address Descriptor (VAD) Tree**.

- Every `EPROCESS` contains a pointer `VadRoot` pointing to the root `_MMVAD` node.
- Each `_MMVAD` node defines:
  - `StartingVpn` & `EndingVpn`: Starting/ending Virtual Page Numbers.
  - `Protection`: Initial allocation protection (`PAGE_READWRITE`, `PAGE_EXECUTE_READWRITE`).
  - `PrivateMemory`: Flag indicating if memory is private or backed by a Section/File mapping.

---

## 4. Security Implications & Memory Protections

- **Data Execution Prevention (DEP / NX Bit)**: Hardware page table bit (Bit 63: `No-Execute`) enforced by CPU. If EIP/RIP attempts to execute code from a page marked `NX=1`, the CPU throws a Page Fault (`#PF`), preventing shellcode execution in stack or heap.
- **Page Protection Constants**:
  - `PAGE_NOACCESS` (`0x01`): Guard pages / null pointers.
  - `PAGE_READONLY` (`0x02`): Code sections (`.text`).
  - `PAGE_READWRITE` (`0x04`): Data sections, heap, stack.
  - `PAGE_EXECUTE_READWRITE` (`0x40`): Executable and writable memory (High-risk target; indicator of shellcode allocation).

---

## 5. Attack Vectors & Exploitation Primitives

1. **Shellcode Execution via `PAGE_EXECUTE_READWRITE` (RWX)**:
   Allocating memory via `VirtualAlloc(..., PAGE_EXECUTE_READWRITE)`, copying shellcode payload, and creating a thread (`CreateThread` / `RtlCreateUserThread`).
2. **Process Injection (Reflective DLL Injection)**:
   Using `VirtualAllocEx` and `WriteProcessMemory` to inject a DLL into a target process address space, bypassing standard Windows loader (`LoadLibrary`) disk artifacts.
3. **VAD Tree Tampering**:
   Manipulating `_MMVAD` node protection flags in kernel memory to hide executable allocations from user-mode security tools (`VirtualQueryEx` API hooks).

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Sysmon Event ID 8**: CreateRemoteThread (detects remote thread creation across processes).
- **Sysmon Event ID 10**: ProcessAccess (detects `VirtualAllocEx` / `WriteProcessMemory` access attempts).
- **Kernel ETW Provider**: `Microsoft-Windows-Threat-Intelligence` (`{F4E1897C-B9DD-46CB-825C-A77E86798292}`) - logs `KERNEL_THREATINT_VIRTUAL_ALLOC`.

### Sigma Detection Rule Snippet (Remote RWX Memory Allocation):
```yaml
title: Remote RWX Memory Allocation in Target Process
id: 5410928f-721a-4d2b-a12b-312901fa41bc
status: experimental
description: Detects VirtualAllocEx calls granting PAGE_EXECUTE_READWRITE permissions in target processes.
logsource:
  category: process_access
  product: windows
detection:
  selection:
    GrantedAccess|contains: '0x0020' # PROCESS_VM_OPERATION
    CallTrace|contains: 'VirtualAllocEx'
  condition: selection
level: high
```

---

## 7. Engineering & Hands-On Implementation

### Inspecting VAD Tree in WinDbg:
```text
kd> !vad ffffe001ab387080
VAD     level  start      end        commit
ffffe001ab412340 ( 3)    7ff72d000  7ff72d00f     1 Private READWRITE
ffffe001ab567890 ( 2)    7ff72d010  7ff72d1ff   512 Mapped  READONLY   \Device\HarddiskVolume3\Windows\System32\ntdll.dll

kd> !pte 0x00007ff72d000000
VA 00007ff72d000000
PXE at FFFF6B85C2E17000    PPE at FFFF6B85C2E14000    PDE at FFFF6B85C2800000    PTE at FFFF6B8500000000
contains 00000001A3B45067  contains 00000001A3B46067  contains 00000001A3B47067  contains 80000001A3B48863
pfn 1a3b48    -G-DA--KWV  PAGE_READWRITE
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Application crashes with `0xC0000005` (`ACCESS_VIOLATION`). | Thread attempted to read/write/execute an invalid or protected virtual address. | Inspect crash dump in WinDbg using `!analyze -v` to check RIP and PTE flags. |
| Memory usage spikes in Non-Paged Pool. | Kernel memory leak in driver (`ExAllocatePoolWithTag`). | Run `poolmon.exe` or WinDbg `!poolused 2` to identify leaking pool tag. |

---

## 9. References
- Mark Russinovich, Pavel Yosifovich, Alex Ionescu, *Windows Internals, Part 1 (7th Edition)*.
- Intel 64 and IA-32 Architectures Software Developer's Manual, *Volume 3A: System Programming Guide (Paging Architectures)*.
