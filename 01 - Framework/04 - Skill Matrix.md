# Skill Intelligence Matrix

> **Status:** Active Master Skill Matrix  
> **Version:** 2.0  
> **Last Updated:** 2026-07-28  
> **Framework Standard:** Cyber Act Universal Engineering Framework

---

# Purpose

The Skill Intelligence Matrix is the master tracking system for every technical skill required across the Cyber Act curriculum.

Every skill exists only once in this document.

Every learning module, Cyber Act topic, flagship project, HTB room, portfolio artifact, interview topic, resume bullet, and open-source contribution references the skills defined here.

---

# Skill Lifecycle States

```text
Not Started ──► Learning ──► Practicing ──► Implemented ──► Documented ──► Interview Ready ──► Placement Ready
```

- **Not Started:** Skill identified in curriculum but not yet studied.
- **Learning:** Currently studying theory, architecture, and documentation.
- **Practicing:** Executing CLI commands, lab exercises, and trace tools.
- **Implemented:** Applied skill in project code, tooling, or automation scripts.
- **Documented:** Documented in Cyber Act Vault following the 9-Part Framework.
- **Interview Ready:** Can answer architecture, security, and troubleshooting interview questions.
- **Placement Ready:** Demonstrated in portfolio projects, resume bullet points, and live technical interviews.

---

# Master Skill Inventory

## 1. Development & Version Control

| Skill ID | Skill Name | Domain | Priority | Lifecycle Status | Target Modules |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **SK-DEV-001** | Git Version Control & Branching | Systems / Dev | P0 | **Interview Ready** | `P-01` |
| **SK-DEV-002** | Git Internals (Blobs, Trees, Commits) | Systems / Dev | P0 | **Interview Ready** | `P-01` |
| **SK-DEV-003** | Python Systems & Socket Scripting | Systems / Dev | P0 | **Interview Ready** | `P-04` |
| **SK-DEV-004** | Python CTypes & Subprocess Automation | Systems / Dev | P1 | **Interview Ready** | `P-04` |
| **SK-DEV-005** | Relational Database & SQL Engineering | Systems / Dev | P0 | **Interview Ready** | `P-05` |
| **SK-DEV-006** | SQL Injection Defense & Parameterization | SecEng / Dev | P0 | **Interview Ready** | `P-05` |

---

## 2. Operating Systems & Kernel Internals

| Skill ID | Skill Name | Domain | Priority | Lifecycle Status | Target Modules |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **SK-OS-001** | Linux Administration & POSIX Permissions | Linux OS | P0 | **Interview Ready** | `P-02` |
| **SK-OS-002** | Virtual Filesystem (VFS) & Inodes | Linux OS | P0 | **Interview Ready** | `P-10` |
| **SK-OS-003** | Linux Process Architecture (`task_struct`, `fork`) | Linux OS | P0 | **Interview Ready** | `P-07/08` |
| **SK-OS-004** | Virtual Memory Paging & MMU Mechanics | Linux OS | P0 | **Interview Ready** | `P-09` |
| **SK-OS-005** | ASLR & DEP/NX Exploit Mitigations | SecEng / OS | P0 | **Interview Ready** | `P-09` |
| **SK-OS-006** | Linux Namespaces & cgroups Isolation | Containers / OS | P0 | **Interview Ready** | `P-14` |
| **SK-OS-007** | Linux System Call Interface & eBPF | Kernel / OS | P1 | **Interview Ready** | `P-14` |
| **SK-OS-008** | Windows Registry & Service Subsystem | Windows OS | P0 | **Interview Ready** | `P-03` |
| **SK-OS-009** | Windows Access Tokens & DACLs | Security / OS | P0 | **Interview Ready** | `P-03/15` |
| **SK-OS-010** | LSASS & Virtualization-Based Security (VBS) | Security / OS | P0 | **Interview Ready** | `P-15` |

---

## 3. Web Protocols & Networking

| Skill ID | Skill Name | Domain | Priority | Lifecycle Status | Target Modules |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **SK-NET-001** | HTTP/1.1 & HTTP/2 Stateless Protocol Analysis | Networking | P0 | **Interview Ready** | `P-06` |
| **SK-NET-002** | RESTful API Architecture & Security Headers | Web / Sec | P0 | **Interview Ready** | `P-06` |
| **SK-NET-003** | TCP/IP 4-Layer Stack & 3-Way Handshake | Networking | P0 | **Interview Ready** | `P-11` |
| **SK-NET-004** | Packet Capture Analysis (`tcpdump`, Wireshark) | SecOps / Net | P0 | **Interview Ready** | `P-11` |

---

## 4. Authentication, Telemetry & Security Operations

| Skill ID | Skill Name | Domain | Priority | Lifecycle Status | Target Modules |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **SK-SEC-001** | Password Hashing (Argon2id, bcrypt) | Security / Auth | P0 | **Interview Ready** | `P-12` |
| **SK-SEC-002** | Kerberos Authentication (AS, TGS, TGT) | Security / Auth | P0 | **Interview Ready** | `P-12` |
| **SK-SEC-003** | JWT Token Structure & Security Validation | Web / Auth | P0 | **Interview Ready** | `P-12` |
| **SK-SEC-004** | Sysmon & Windows Event Log Analytics | Detection Eng | P0 | **Interview Ready** | `P-13` |
| **SK-SEC-005** | Linux Auditd System Call Logging | Detection Eng | P0 | **Interview Ready** | `P-13` |
| **SK-SEC-006** | Structured JSON Logging & SIEM Ingestion | SecOps / Telemetry| P0 | **Interview Ready** | `P-13` |

---

# Stage Completion Summary

| Skill Domain | Total Skills | Documented | Interview Ready | Placement Ready |
| :--- | :---: | :---: | :---: | :---: |
| **Development & Version Control** | 6 | 6 | 6 | 0 |
| **Operating Systems & Kernels** | 10 | 10 | 10 | 0 |
| **Networking & Web Protocols** | 4 | 4 | 4 | 0 |
| **Authentication & Telemetry** | 6 | 6 | 6 | 0 |
| **Total Stage 0 Skills** | **26** | **26 (100%)** | **26 (100%)** | **0** |
