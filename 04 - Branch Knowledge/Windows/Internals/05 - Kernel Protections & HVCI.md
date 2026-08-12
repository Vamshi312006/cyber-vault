---
id: "mod-win-kernel-mitigations"
title: "Windows Kernel Security Protections & Virtualization-Based Security (VBS)"
domain: "Domain-01"
branch: "windows-internals"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Kernel Security Protections & Virtualization-Based Security (VBS)

## 1. Overview & Purpose
Modern Windows kernels implement layered hardware and hypervisor-enforced protection boundaries designed to prevent unauthorized kernel modifications, driver tampering, and credential extraction.

This module details Kernel Patch Protection (KPP / PatchGuard), Driver Signature Enforcement (DSE), Virtualization-Based Security (VBS), Hypervisor-Protected Code Integrity (HVCI), Control Flow Guard (CFG/XFG), and Credential Guard. Understanding these mitigations is essential for analyzing kernel exploitation limits, BYOVD attacks, and hypervisor-level security boundaries.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Hardware Layer (CPU / SLAT / VT-x)
        HYPERVISOR["Hyper-V Hypervisor (VMM / Ring -1)"]
    end

    subgraph Virtual Trust Levels (VTL)
        subgraph VTL 0 (Normal World)
            U_MODE0["User Mode (Ring 3)<br/>Apps, Services"]
            K_MODE0["Kernel Mode (Ring 0)<br/>NTOSKRNL, Drivers, KPP"]
        end

        subgraph VTL 1 (Secure World - Isolated Memory)
            SKERNEL["Secure Kernel (securekernel.exe)<br/>HVCI Enforcement Engine"]
            LSAISO["Isolated LSA (lsaiso.exe)<br/>Credential Guard Storage"]
        end
    end

    HYPERVISOR --> VTL0
    HYPERVISOR --> VTL1
    VTL0 -.->|Secure Service Call - VMCALL| VTL1
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 Kernel Patch Protection (KPP / PatchGuard)
Introduced in 64-bit Windows, PatchGuard is a set of randomized, encrypted kernel routines that periodically execute to verify the integrity of critical kernel structures.

#### Structures Monitored by PatchGuard:
- System Service Descriptor Table (SSDT) modifications.
- Global Descriptor Table (GDT) and Interrupt Descriptor Table (IDT).
- Kernel code section (`NTOSKRNL.EXE` `.text` modification).
- Critical MSR registers (`IA32_LSTAR` system call handler).

If PatchGuard detects unauthorized modification, it forces an immediate bugcheck: `CRITICAL_STRUCTURE_CORRUPTION` (`0x00000109`).

---

### 3.2 Driver Signature Enforcement (DSE)
Driver Signature Enforcement requires all 64-bit kernel-mode drivers (`.sys` files) to be digitally signed by a trusted Certificate Authority (and WHQL certified by Microsoft).
- Enforced during `NtLoadDriver` call by checking `g_CiEnabled` global variable in `CI.DLL` (Code Integrity module).
- Disabling DSE programmatically requires kernel write primitives to flip `g_CiEnabled` or `g_CiOptions` in kernel memory.

---

### 3.3 Virtualization-Based Security (VBS) & HVCI
VBS leverages Hyper-V virtualization extensions (Intel VT-x / AMD-V and Second Level Address Translation - SLAT / EPT) to partition memory into two Virtual Trust Levels (VTL):
- **VTL 0 (Normal World)**: Standard Windows OS, Ring 0 kernel, and Ring 3 user apps.
- **VTL 1 (Secure World)**: Isolated Secure Kernel (`securekernel.exe`) isolated via hardware EPT page tables.

#### Hypervisor-Protected Code Integrity (HVCI / Memory Integrity):
HVCI runs inside VTL 1. It enforces W^X (Write XOR Execute) memory policies on VTL 0 kernel memory using EPT page table bits. Even if a Ring 0 kernel exploit gains arbitrary write access in VTL 0, it **cannot make kernel memory executable** without VTL 1 validation.

#### Credential Guard:
Extracts the Kerberos ticket-granting ticket (TGT) storage and NTLM hash processing out of `lsass.exe` (VTL 0) into `lsaiso.exe` (VTL 1). Dumping `lsass.exe` memory from VTL 0 yields no plaintext credentials or hash secrets.

---

## 4. Security Implications & Boundary Controls

- **Ring 0 is no longer absolute**: Under VBS/HVCI, Ring 0 (kernel mode) is subordinate to Ring -1 (Hypervisor) and VTL 1 (Secure World).
- **BYOVD (Bring Your Own Vulnerable Driver)**: Because direct kernel code modification is blocked by DSE and HVCI, adversaries load legitimately signed but vulnerable third-party drivers (e.g., `gdrv.sys`, `mhyprot2.sys`) to leverage driver IOCTL vulnerabilities for kernel memory read/write.

---

## 5. Attack Vectors & Mitigation Bypasses

1. **BYOVD Exploitation**:
   Loading a signed vulnerable driver via `SeLoadDriverPrivilege`, then sending crafted IOCTLs to perform physical/virtual memory read and write primitives.
2. **DSE Disabling via Vulnerable Drivers (EOP/BYOVD)**:
   Leveraging a driver's arbitrary write primitive to modify `g_CiOptions` in kernel memory, temporarily disabling DSE to load an unsigned rootkit driver.
3. **Control Flow Guard (CFG) / XFG Bypasses**:
   Crafting ROP (Return-Oriented Programming) chains targeting indirect call sites that lack bitmap validation checks.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Sysmon Event ID 6**: Driver Loaded (captures driver path, signature status, hashes).
- **Windows Security Event ID 5038**: Code Integrity determined that the image hash of a file is not valid.
- **Windows Security Event ID 7045**: A service was installed in the system (kernel driver service).

### Sigma Detection Rule Snippet (Vulnerable Driver Load Attempt):
```yaml
title: Known Vulnerable Driver Loading Attempt (BYOVD)
id: f481902a-921a-4d2b-a01b-829101fa51bc
status: experimental
description: Detects loading of known vulnerable signed drivers used in BYOVD attacks.
logsource:
  category: driver_load
  product: windows
detection:
  selection:
    Hashes|contains:
      - 'IMPHASH_GDRV_SYS'
      - 'IMPHASH_MHYPROT2'
    ImageLoaded|endswith:
      - '\gdrv.sys'
      - '\mhyprot2.sys'
  condition: selection
level: high
```

---

## 7. Engineering & Hands-On Implementation

### Verifying VBS and HVCI Status in Windows:
```powershell
# PowerShell VBS / HVCI Audit Command
Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard | 
Select-Object SecurityServicesConfigured, SecurityServicesRunning, VirtualizationBasedSecurityStatus
```

### Output Interpretation:
- `VirtualizationBasedSecurityStatus = 2`: VBS Running.
- `SecurityServicesRunning = {1, 2}`: Credential Guard (1) and HVCI (2) active.

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| System crashes with `PAGE_FAULT_IN_NONPAGED_AREA` after loading custom driver. | HVCI blocked executable page allocation in VTL 0 kernel space. | Recompile driver with `/INTEGRITYCHECK` and ensure no RWX memory pages are requested. |
| Mimikatz `sekurlsa::logonpasswords` fails to extract hashes. | Credential Guard (VTL 1 isolation) active on host. | Verify `lsaiso.exe` process presence. Plaintext extraction disabled by design. |

---

## 9. References
- Microsoft Learn: *Virtualization-Based Security (VBS) & Hypervisor-Enforced Code Integrity (HVCI)*.
- Pavel Yosifovich, *Windows Kernel Programming (2nd Edition)*.
