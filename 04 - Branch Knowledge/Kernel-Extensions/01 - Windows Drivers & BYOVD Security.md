---
id: "mod-drv-win-irp"
title: "Windows Driver Architecture, IOCTLs & BYOVD Exploitation"
domain: "Domain-01"
branch: "driver-security"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Driver Architecture, IOCTLs & BYOVD Exploitation

## 1. Overview & Purpose
Windows kernel-mode drivers (`.sys` files) execute with Ring 0 privileges. When a driver contains security flaws in its I/O control dispatch routines, attackers can exploit it to read and write arbitrary kernel memory—a technique known as **Bring Your Own Vulnerable Driver (BYOVD)**.

This module details Windows Driver Frameworks (WDM / KMDF), I/O Request Packets (IRPs), I/O Control Codes (IOCTLs), Direct vs Buffered I/O memory handling, and BYOVD exploitation techniques used by modern threat actors to disable EDR drivers and Kernel Patch Protection.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph User Space (Ring 3)
        APP["Malware / Exploitation Process"]
        CREATE_FILE["CreateFileW('\\\\.\\VulnerableDriverDevice')<br/>Obtains Device Handle"]
        DEVICE_CONTROL["DeviceIoControl(Handle, IOCTL_CODE, InBuf, OutBuf...)"]

        APP --> CREATE_FILE
        CREATE_FILE --> DEVICE_CONTROL
    end

    subgraph Ring 0 (Kernel Space - Vulnerable Driver Execution)
        DRIVER_OBJECT["DRIVER_OBJECT (DriverEntry)"]
        MAJOR_FUNCTION["MajorFunction[IRP_MJ_DEVICE_CONTROL]"]
        IOCTL_HANDLER["Vulnerable IOCTL Dispatch Routine<br/>(Reads/Writes arbitrary physical/virtual addresses)"]

        DRIVER_OBJECT --> MAJOR_FUNCTION
        MAJOR_FUNCTION --> IOCTL_HANDLER
    end

    DEVICE_CONTROL -.->|Kernel IRP Dispatch| MAJOR_FUNCTION
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 Windows Driver Dispatch Pipeline
1. **Driver Initialization**: Driver implements `DriverEntry(PDRIVER_OBJECT DriverObject, PUNICODE_STRING RegistryPath)`.
2. **Device Creation**: Creates a named device object (`IoCreateDevice`) and symbolic link (`IoCreateSymbolicLink`) to allow user-mode applications to open handles to `\\.\DeviceName`.
3. **IRP Registration**: Registers function pointers in `DriverObject->MajorFunction`:
   - `MajorFunction[IRP_MJ_CREATE]`: Called when `CreateFile` opens device.
   - `MajorFunction[IRP_MJ_DEVICE_CONTROL]`: Called when `DeviceIoControl` sends an IOCTL packet.

---

### 3.2 Anatomy of an IOCTL Code
An I/O Control Code (IOCTL) is a 32-bit integer constructed via `CTL_CODE` macro:

```text
 31 30        16 15 14 13 12            2 1 0
┌──┬────────────┬──┬──┬──────────────────┬────┐
│  │ DeviceType │  │  │   Function Code  │Transfer│
│  │  (16 bits) │  │  │     (12 bits)    │ Type  │
└──┴────────────┴──┴──┴──────────────────┴────┘
```

#### Transfer Types:
- `METHOD_BUFFERED` (`0`): Kernel copies user input/output buffers into non-paged pool memory (Safe).
- `METHOD_IN_DIRECT` / `METHOD_OUT_DIRECT`: Kernel verifies memory pages via Memory Descriptor Lists (MDL).
- `METHOD_NEITHER` (`3`): Driver receives raw user-mode pointers without kernel validation (**High Risk for Arbitrary Read/Write Exploitation**).

---

## 4. Security Implications & BYOVD Attacks

- **BYOVD Threat Model**: Because Driver Signature Enforcement (DSE) blocks un-signed drivers, attackers drop a **legitimately signed, vulnerable third-party driver** (e.g., hardware monitoring utilities, anti-cheat drivers).
- **EDR Termination**: Once loaded, the attacker uses the driver's vulnerable IOCTL to write to the `EPROCESS` or unhook active EDR kernel callbacks (`PspCreateProcessNotifyRoutine`, `ObRegisterCallbacks`).

---

## 5. Attack Vectors & Exploitation Primitives

1. **Arbitrary Physical/Virtual Memory Read/Write via IOCTL**:
   Exploiting `METHOD_NEITHER` or unvalidated pointer arguments inside a driver's IOCTL handler to read or write arbitrary addresses in kernel space.
2. **Disabling Driver Signature Enforcement (`g_CiOptions`)**:
   Leveraging a BYOVD write primitive to flip `g_CiOptions` in `CI.DLL` to `0`, allowing the loading of unsigned kernel rootkits.
3. **Un-registering EDR Telemetry Callbacks**:
   Locating the `PspCreateProcessNotifyRoutine` array in kernel memory and overwriting EDR callback function pointers with `NULL` or `RET` bytes (`0xC3`).

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Sysmon Event ID 6**: Driver Loaded (captures driver path, signature status, hashes).
- **Windows Event ID 7045**: Service Installation (Kernel Driver Service).
- **Microsoft Vulnerable Driver Blocklist**: Enforced via HVCI or Windows Defender to block known vulnerable driver hashes from loading.

---

## 7. Engineering & Hands-On Implementation

### Auditing Driver Devices via WinDbg:
```text
kd> !drvobj \Driver\VulnerableDriver 2
Driver object (ffffeb0123456780) is for:
 \Driver\VulnerableDriver
DriverEntry:   fffff801`99901000
MajorFunction:
  IRP_MJ_CREATE: fffff801`99901120
  IRP_MJ_DEVICE_CONTROL: fffff801`99901240  <- Inspect IOCTL handler
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| `CreateFile()` returns `ERROR_FILE_NOT_FOUND` (`2`). | Driver symbolic link not created or device permissions restrict user access. | Verify symbolic link in WinObj or check device ACLs. |
| System blue screens with `PAGE_FAULT_IN_NONPAGED_AREA` during `DeviceIoControl`. | Driver dereferenced invalid user pointer under `METHOD_NEITHER`. | Wrap user memory access inside `ProbeForRead` / `ProbeForWrite` and `__try / __except` blocks. |

---

## 9. References
- Pavel Yosifovich, *Windows Kernel Programming (2nd Edition)*.
- LOLDrivers Project: *Catalog of Vulnerable and Malicious Windows Drivers*.
