---
id: "mod-win-process-object"
title: "Windows Process Architecture & Object Manager Internals"
domain: "Domain-01"
branch: "windows-internals"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Process Architecture & Object Manager Internals

## 1. Overview & Purpose
A Windows process is not an executable entity itself; it is an opaque kernel container holding an address space, handle table, access token, threads, and security context.

This module provides deep engineering coverage of the Windows Object Manager, `EPROCESS` and `KPROCESS` kernel structures, `ETHREAD` and `KTHREAD` execution primitives, and handle table resolution mechanics. Understanding these structures is foundational for kernel telemetry validation, process injection detection, and kernel memory forensics.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Kernel Space (Ring 0)
        EPROC["EPROCESS Kernel Structure<br/>(Executive Process Block)"]
        KPROC["KPROCESS Sub-Structure<br/>(Kernel Process Block - Dispatcher/Directory Table Base)"]
        HTABLE["Handle Table<br/>(HANDLE_TABLE -> Object Headers)"]
        TOKEN["Access Token<br/>(EX_FAST_REF -> TOKEN)"]
        ETHREAD1["ETHREAD 1"]
        ETHREAD2["ETHREAD 2"]

        EPROC --> KPROC
        EPROC --> HTABLE
        EPROC --> TOKEN
        EPROC --> ETHREAD1
        EPROC --> ETHREAD2
    end

    subgraph User Space (Ring 3)
        PEB["Process Environment Block<br/>(PEB - Modules, Heap, Environment)"]
        TEB1["Thread Environment Block 1 (TEB)"]
        TEB2["Thread Environment Block 2 (TEB)"]
    end

    EPROC -.->|Peb Pointer| PEB
    ETHREAD1 -.->|Teb Pointer| TEB1
    ETHREAD2 -.->|Teb Pointer| TEB2
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 The `EPROCESS` Structure
The Executive Process Block (`EPROCESS`) is the primary kernel data structure representing a process. Allocated in non-paged pool memory, it contains process identification, security context, handle tables, and links to child threads.

#### Key `EPROCESS` Fields (x64 Windows 11 / Server 2022):
- `+0x000 Pcb : _KPROCESS`: Embedded Kernel Process Block (contains `DirectoryTableBase` / `CR3` page table pointer).
- `+0x440 ProcessId : PVOID`: Unique Process Identifier (PID).
- `+0x448 ActiveProcessLinks : _LIST_ENTRY`: Doubly linked list connecting all active `EPROCESS` blocks in kernel memory (`PsActiveProcessHead`).
- `+0x4b8 Token : _EX_FAST_REF`: Fast reference pointer to the process primary `TOKEN` structure.
- `+0x570 ObjectTable : _HANDLE_TABLE*`: Pointer to the process handle table storing pointers to kernel objects.
- `+0x550 Peb : _PEB*`: Pointer to the user-mode Process Environment Block.
- `+0x5a8 ImageFileName : [15] UCHAR`: ASCII image file name string (14 chars max + null terminator).

---

### 3.2 The Object Manager & Handle Resolution
The Windows Object Manager standardizes the creation, management, security, and lifetime of kernel resources (Files, Processes, Threads, Sections, Mutexes, Events, Registry Keys).

#### Object Header & Body Layout:
```
┌───────────────────────────────────────────────────────────┐
│ OBJECT_HEADER_QUOTA_INFO (Optional Pre-Header)           │
├───────────────────────────────────────────────────────────┤
│ OBJECT_HEADER_NAME_INFO  (Optional Pre-Header)           │
├───────────────────────────────────────────────────────────┤
│ OBJECT_HEADER                                             │
│  ├── PointerCount  : Active kernel references            │
│  ├── HandleCount   : Open user-mode handles              │
│  ├── TypeIndex     : Index into ObTypeIndexTable          │
│  └── SecurityDescriptor : Security Descriptor Pointer      │
├───────────────────────────────────────────────────────────┤
│ OBJECT BODY (e.g., EPROCESS, FILE_OBJECT, SECTION)        │
└───────────────────────────────────────────────────────────┘
```

#### Handle Lookup Algorithm:
When a user-mode application invokes a system call passing a `HANDLE` (e.g., `0x44`), the kernel resolves it as follows:
1. `HANDLE` value is divided by 4 (strip lower flags).
2. The kernel locates the process `OBJECT_TABLE` (`HANDLE_TABLE`).
3. Multilevel handle table lookup converts index into a `HANDLE_TABLE_ENTRY`.
4. The entry yields the `OBJECT_HEADER` pointer and granted access mask (`GrantedAccess`).
5. Access mask is checked against requested access. If permitted, the object body pointer is returned.

---

## 4. Security Implications & Boundary Controls
- **Process Isolation**: The page directory base register (`DirectoryTableBase` / `CR3`) isolates address spaces between processes. Memory access across processes requires explicit handle allocation with `PROCESS_VM_READ` or `PROCESS_VM_WRITE` rights.
- **DKOM (Direct Kernel Object Manipulation)**: Malicious drivers or kernel exploits can un-link `ActiveProcessLinks` to hide processes from standard enumeration tools (`EnumProcesses`, `tasklist`) while leaving execution intact on the dispatcher scheduler queues.

---

## 5. Attack Vectors & Exploitation Primitives

1. **Process Unlinking (DKOM)**:
   Modifying `EPROCESS->ActiveProcessLinks.Flink` and `Blink` pointers to remove a target `EPROCESS` node from `PsActiveProcessHead`.
2. **Process Herpaderping / Doppelgänging**:
   Manipulating image section objects and file handles before/during process creation to obscure the executable image backed on disk from security scanners.
3. **Handle Duplication & Elevation**:
   Opening a handle to a high-privileged process (e.g., `lsass.exe` or `winlogon.exe`) using `PROCESS_CREATE_PROCESS` or `PROCESS_DUP_HANDLE` to spawn child processes within a privileged security context.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Sysmon Event ID 1**: Process Creation (captures PID, ParentPID, ImagePath, CommandLine, Hashes, User).
- **Windows Event Log ID 4688**: A new process has been created (requires Audit Process Creation policy).
- **Kernel ETW Provider**: `Microsoft-Windows-Kernel-Process` (`{22FB2CD6-0E7B-422B-A0C7-2FAD1FD0E716}`).

### Sigma Detection Rule Snippet (Process Creation from Unusual Parent):
```yaml
title: Suspicious LSASS Child Process Creation
id: c382910a-81a2-4632-a2fb-832101fa21bc
status: experimental
description: Detects process creation spawned directly by LSASS (unusual parent behavior).
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    ParentImage|endswith: '\lsass.exe'
  filter:
    Image|endswith: '\werfault.exe'
  condition: selection and not filter
falsepositives:
  - Windows Error Reporting (WerFault) during crashes.
level: high
```

---

## 7. Engineering & Hands-On Implementation

### Inspecting `EPROCESS` in WinDbg (Kernel Debugging):
```text
kd> !process 0 0 lsass.exe
PROCESS ffffe001ab387080
    SessionId: 0  Cid: 02a4    Peb: 7ff72d000  ParentCid: 0258
    DirBase: 1a3b4000  ObjectTable: ffffc00012345678  ImageFileName: lsass.exe

kd> dt _EPROCESS ffffe001ab387080 ProcessId ActiveProcessLinks Token ObjectTable ImageFileName
nt!_EPROCESS
   +0x440 ProcessId           : 0x00000000`000002a4 Void
   +0x448 ActiveProcessLinks  : _LIST_ENTRY [ 0xffffe001`ab3980c8 - 0xffffe001`ab3760c8 ]
   +0x4b8 Token               : _EX_FAST_REF
   +0x570 ObjectTable         : 0xffffc000`12345678 _HANDLE_TABLE
   +0x5a8 ImageFileName       : [15]  "lsass.exe"
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Process visible in WinDbg scheduler but missing in Process Explorer. | Process unlinked via DKOM (`ActiveProcessLinks`). | Run `!process 0 7` in WinDbg to cross-check dispatcher queues against linked lists. |
| `OpenProcess()` returns `STATUS_ACCESS_DENIED` (`0xC0000022`). | Target process Protected Process Light (PPL) or handle rights insufficient. | Check `EPROCESS->Protection` flags or request reduced access mask (`PROCESS_QUERY_LIMITED_INFORMATION`). |

---

## 9. References
- Mark Russinovich, Pavel Yosifovich, Alex Ionescu, *Windows Internals, Part 1 (7th Edition)*.
- Microsoft Learn: *Object Manager Architecture & Kernel Data Structures*.
- Cyber Act Framework: `01 - Framework/Cyber Act Framework/02 - Universal Engineering Framework.md`.
