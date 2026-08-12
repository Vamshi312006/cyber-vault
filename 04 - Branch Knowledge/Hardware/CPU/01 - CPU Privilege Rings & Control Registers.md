---
id: "mod-cpu-rings-isolation"
title: "CPU Privilege Rings, Control Registers & Hardware Isolation"
domain: "Domain-01"
branch: "memory-architecture"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# CPU Privilege Rings, Control Registers & Hardware Isolation

## 1. Overview & Purpose
Hardware CPU privilege rings and control registers form the foundational boundary separating unprivileged software code from full hardware execution control.

This module details x86_64 CPU privilege rings (Ring 0 vs Ring 3, Ring -1 Hypervisor, Ring -2 SMM), Segment Descriptors (GDT / IDT / TSS), Control Registers (`CR0`, `CR3`, `CR4`), Model-Specific Registers (MSRs), and Model-Specific Register security. Understanding hardware CPU primitives is essential for OS kernel engineering, hypervisor security, and low-level vulnerability research.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph CPU Privilege Rings Architecture
        R3["Ring 3: User Mode (Unprivileged Applications)"]
        R0["Ring 0: Kernel Mode (OS Kernel & Device Drivers)"]
        R1["Ring -1: Hypervisor Mode (VMX Root / Hyper-V / KVM)"]
        R2["Ring -2: System Management Mode (SMM / Firmware / BIOS)"]
        R3_SEC["Ring -3: Management Engine (Intel ME / AMD PSP Subprocessor)"]
    end

    subgraph Hardware Control Registers (x86_64)
        CR0["CR0: Protected Mode (PE), Paging (PG), Write Protect (WP)"]
        CR3["CR3: Page Directory Base Register (PML4 Base Address)"]
        CR4["CR4: PAE, SMEP (bit 20), SMAP (bit 21), CET (bit 23)"]
        MSR_LSTAR["MSR 0xC0000082 (IA32_LSTAR - SYSCALL Entry Point)"]
    end

    R3 -->|SYSCALL Instruction| R0
    R0 -->|VMCALL Instruction| R1
    R1 -->|SMI Interrupt| R2
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 The CPU Privilege Ring Hierarchy
- **Ring 3 (User Mode)**: Code executes with restricted instruction sets. Privileged instructions (`cli`, `sti`, `in`, `out`, `mov cr3`) trigger a CPU General Protection Fault (`#GP`).
- **Ring 0 (Kernel Mode)**: Full hardware control. Can execute all CPU instructions and modify control registers.
- **Ring -1 (Hypervisor Mode - VMX Root)**: Introduced by Intel VT-x and AMD-V. Manages virtual machines via Extended Page Tables (EPT) and `VMCALL` transitions.
- **Ring -2 (System Management Mode - SMM)**: Highest x86 execution tier. Activated via System Management Interrupt (`SMI`). Operates in dedicated SMRAM memory inaccessible even to Ring 0 kernel code.
- **Ring -3 (Subprocessor Execution)**: Embedded hardware coprocessor (Intel Management Engine / AMD Platform Security Processor) operating below the primary CPU.

---

### 3.2 Key Hardware Control Registers
1. **`CR0` (Control Register 0)**:
   - `PE` (Bit 0): Protected Mode Enable.
   - `WP` (Bit 16): Write Protect. When set, CPU prevents Ring 0 kernel code from writing to read-only pages.
   - `PG` (Bit 31): Paging Enable. Activates MMU page table address translation.
2. **`CR3` (Control Register 3 / Page Directory Base Register)**: Holds the physical base address of the active process PML4 page table.
3. **`CR4` (Control Register 4)**:
   - `SMEP` (Bit 20): Supervisor Mode Execution Prevention.
   - `SMAP` (Bit 21): Supervisor Mode Access Prevention.
   - `CET` (Bit 23): Control-Flow Enforcement Technology.

---

### 3.3 Descriptor Tables (GDT, IDT, TSS)
- **Global Descriptor Table (GDT)**: Defines memory segment attributes and privilege levels (CS / DS / SS selectors).
- **Interrupt Descriptor Table (IDT)**: Maps interrupt vector numbers (`0x00` - `0xFF`) to kernel interrupt service routines (`#GP`, `#PF`, `#DB`).
- **Task State Segment (TSS)**: Holds Kernel Stack Pointers (`RSP0`) used when transitioning from Ring 3 to Ring 0 during interrupts or syscalls.

---

## 4. Security Implications & Boundary Controls

- **Hardware Enforced Isolation**: CPU hardware verifies Privilege Level (`CPL`) stored in the CS segment register against Target Descriptor Privilege Level (`DPL`) on every memory access.
- **MSR Protection**: Model-Specific Registers (MSRs) control critical CPU execution paths (e.g., `IA32_LSTAR` for system call entry). Modifying MSRs requires Ring 0 privileges (`wrmsr` instruction).

---

## 5. Attack Vectors & Exploitation Primitives

1. **`CR0.WP` Flipping (Legacy Kernel Rootkits)**:
   Disabling Write Protect by clearing Bit 16 in `CR0` (`mov cr0, eax`), allowing Ring 0 code to overwrite read-only kernel code (`NTOSKRNL` or `sys_call_table`).
2. **SMM Firmware Exploitation (SMM Rootkits)**:
   Triggering SMI interrupts with crafted memory pointers to exploit vulnerabilities inside System Management Mode (SMRAM), achieving persistence that survives OS reinstalls.
3. **`IA32_LSTAR` MSR Overwrite**:
   Writing an attacker-controlled address to `IA32_LSTAR` via `wrmsr` to hijack all incoming `SYSCALL` transitions across the operating system.

---

## 6. Defense & Telemetry Verification

### Hardware-Level Enforcement:
- **Hypervisor Guarding of MSRs**: Under Hyper-V / VBS, the hypervisor intercepts `wrmsr` calls to `IA32_LSTAR` and `CR4`, preventing Ring 0 kernel exploits from disabling hardware mitigations.
- **Secure Boot & Firmware Measurement**: TPM 2.0 PCR registers measure GDT/IDT and BIOS/SMM firmware hashes before CPU boot execution.

---

## 7. Engineering & Hands-On Implementation

### Inspecting Control Registers in WinDbg:
```text
kd> r cr0, cr3, cr4
cr0=0000000080050033 cr3=00000001a3b40000 cr4=00000000003706f0

kd> !gdtr
GDT Base: fffff801`2a815000, Limit: 007f
Segment 0010: Base 00000000, Limit ffffffff, Type Code, DPL 0 (Kernel)
Segment 002b: Base 00000000, Limit ffffffff, Type Data, DPL 3 (User)
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| CPU triggers General Protection Fault (`#GP` / Exception 13). | Executed privileged instruction in Ring 3 or loaded invalid GDT selector. | Inspect RIP and CS register DPL bits in crash dump. |
| System blue screens with `UNEXPECTED_KERNEL_MODE_TRAP`. | Kernel stack overflow or invalid TSS stack pointer (`RSP0`). | Check double fault exception vector (`#DF`) in WinDbg. |

---

## 9. References
- Intel 64 and IA-32 Architectures Software Developer's Manual, *Volume 3A: System Programming Guide*.
- AMD64 Architecture Programmer's Manual, *Volume 2: System Programming*.
