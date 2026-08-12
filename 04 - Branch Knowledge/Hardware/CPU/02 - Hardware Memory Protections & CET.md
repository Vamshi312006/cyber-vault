---
id: "mod-cpu-memory-mitigations"
title: "Hardware Memory Protections, SMEP, SMAP & Intel CET"
domain: "Domain-01"
branch: "memory-architecture"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Hardware Memory Protections, SMEP, SMAP & Intel CET

## 1. Overview & Purpose
Software-level security checks can be bypassed if the underlying CPU hardware permits arbitrary execution transitions across memory space boundaries.

This module details CPU-enforced hardware memory protections: Supervisor Mode Execution Prevention (SMEP), Supervisor Mode Access Prevention (SMAP), Intel Control-Flow Enforcement Technology (CET - Shadow Stack & Indirect Branch Tracking), ARM Pointer Authentication Codes (PAC), and Branch Target Identification (BTI). Understanding these CPU features is essential for analyzing modern kernel exploit constraints and hardware-enforced mitigation engineering.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Hardware Mitigation Boundary (CPU MMU & Pipeline)
        SMEP["SMEP (Supervisor Mode Execution Prevention)<br/>Blocks Ring 0 from EXECUTING code in User Pages"]
        SMAP["SMAP (Supervisor Mode Access Prevention)<br/>Blocks Ring 0 from READING/WRITING User Pages"]
        CET_SS["Intel CET Shadow Stack<br/>Hardware-managed parallel return address stack"]
        CET_IBT["Intel CET Indirect Branch Tracking<br/>Enforces ENDBR64 instruction at indirect jump targets"]
        ARM_PAC["ARM PAC (Pointer Authentication Codes)<br/>Cryptographically signs pointers using QARMA cipher"]
    end

    subgraph CPU Control Registers & Execution Enforcements
        CR4_REG["CR4 Register<br/>├── Bit 20: SMEP Enable<br/>├── Bit 21: SMAP Enable<br/>└── Bit 23: CET Enable"]
    end

    CR4_REG --> SMEP
    CR4_REG --> SMAP
    CR4_REG --> CET_SS
    CR4_REG --> CET_IBT
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 Supervisor Mode Execution Prevention (SMEP)
Before SMEP, kernel exploits (e.g., Ring 0 NULL pointer dereference or stack buffer overflow) would overwrite a kernel function pointer with the address of shellcode placed in user-mode memory. The kernel would then jump to Ring 3 memory and execute shellcode with Ring 0 privileges.

- **SMEP Mechanism**: Controlled by `CR4` Bit 20.
- When `CR4.SMEP = 1`, if the CPU executes code in a page where the `User/Supervisor` page table bit (`U/S = 1`), the MMU triggers an immediate Page Fault (`#PF`) with a hardware error code, halting kernel execution.

---

### 3.2 Supervisor Mode Access Prevention (SMAP)
While SMEP prevents *executing* user-mode code in Ring 0, SMAP prevents kernel code from *reading or writing* data in user-mode memory without explicit permission.

- **SMAP Mechanism**: Controlled by `CR4` Bit 21 and the EFLAGS `AC` (Alignment Check) bit.
- When `CR4.SMAP = 1`, any read or write access to user-space memory (`U/S = 1`) by Ring 0 triggers a Page Fault (`#PF`).
- **`stac` / `clac` Instructions**: Legitimate kernel APIs that copy data to/from user space (`copy_to_user`, `copy_from_user`) temporarily set EFLAGS `AC=1` using `stac` (Set AC Flag) before access, and clear it using `clac` (Clear AC Flag) immediately after.

---

### 3.3 Intel Control-Flow Enforcement Technology (CET)
Intel CET provides hardware mitigation against Return-Oriented Programming (ROP) and Jump-Oriented Programming (JOP):

1. **Shadow Stack (SS)**: A secondary hardware-protected stack used exclusively for return addresses.
   - When a `CALL` instruction executes, the return address is pushed onto both the data stack (`RSP`) and Shadow Stack (`SSP`).
   - When `RET` executes, the CPU compares `[RSP]` against `[SSP]`. If addresses mismatch (indicating ROP tampering), a Control Protection Exception (`#CP`) is thrown.
2. **Indirect Branch Tracking (IBT)**: Prevents JOP attacks by ensuring all indirect calls (`CALL RAX` / `JMP RAX`) jump directly to a valid target starting with an `ENDBR64` instruction.

---

### 3.4 ARM Pointer Authentication Codes (PAC)
On ARM64 (AArch64), PAC inserts a cryptographic signature into the unused upper bits of pointer addresses (bits 48-63) using a secret hardware key (`APIAKey`) and context modifier.
- Before jumping to a pointer, `AUTIA` authenticates the signature.
- If an attacker tampers with the pointer address, authentication fails and the CPU throws an invalid instruction fault.

---

## 4. Security Implications & Exploitation Constraints

- **Elimination of Simple Shellcode Pointers**: SMEP completely neutralizes user-space shellcode execution from kernel mode.
- **Elimination of Traditional ROP**: Intel CET Shadow Stack and ARM PAC render classic return address stack overwrites ineffective at the hardware level.

---

## 5. Attack Vectors & Mitigation Bypasses

1. **Kernel ROP (KROP) to Disable SMEP/SMAP**:
   Because SMEP blocks execution of *user-space* pages, attackers construct Return-Oriented Programming (ROP) chains built entirely from existing *kernel-space* instructions (`NTOSKRNL` or `vmlinux` gadgets) to execute `mov cr4, rax` and flip SMEP/SMAP bits off.
2. **Data-Only Attacks (Non-Control Data Attacks)**:
   Exploiting vulnerabilities to alter critical data fields (e.g., overwriting `struct cred` or `EPROCESS->Token`) without altering control flow, completely bypassing ROP/JOP/CET mitigations.

---

## 6. Defense & Telemetry Verification

### Hardware & Hypervisor Telemetry:
- **Hypervisor CR4 Lock**: VBS / Hyper-V locks `CR4.SMEP` and `CR4.SMAP` bits. Any attempt by Ring 0 code to clear SMEP in `CR4` triggers an immediate VMX trap (`VMEXIT`), terminating the guest VM.

---

## 7. Engineering & Hands-On Implementation

### Verifying SMEP and SMAP Status in Linux:
```bash
# Check CPU flags for smep, smap, and cet (ibt/shstk)
cat /proc/cpuinfo | grep -E "smep|smap|ibt|shstk"
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Kernel Panic: `#PF` in kernel mode accessing user memory. | Kernel driver attempted to read user pointer without `stac` / `clac` instructions under SMAP. | Use proper kernel usercopy wrappers (`copy_from_user()` / `copy_to_user()`). |
| Application crashes with `#CP` exception (Code `0x00000015`). | Intel CET Shadow Stack detected return address mismatch (ROP attack or corrupted stack). | Inspect Control Protection (`#CP`) exception in debugger. |

---

## 9. References
- Intel Corporation, *Control-Flow Enforcement Technology (CET) Architecture Specification*.
- ARM Limited, *ARMv8.3-A Pointer Authentication Architecture*.
