# Cyber Act Module Index

> **Status:** Active Master Index  
> **Version:** 2.0  
> **Last Updated:** 2026-07-28  
> **Framework Standard:** Cyber Act Universal Engineering Framework

---

# Purpose

This document is the master index of every Cyber Act module across all 6 stages.

It provides a single location to track module IDs, priorities, dependencies, duration, implementation status, and project mappings.

> [!NOTE]
> All Stage 0 subtopic notes are stored under `04 - Branch Knowledge/` following the [Universal Topic Template](file:///home/vamshi/Documents/notes/notes1/cyber/01%20-%20Framework/Cyber%20Act%20Framework/03%20-%20Universal%20Topic%20Template.md).  
> For the complete subtopic map, see [00 - Master Branch Knowledge Index.md](file:///home/vamshi/Documents/notes/notes1/cyber/04%20-%20Branch%20Knowledge/00%20-%20Master%20Branch%20Knowledge%20Index.md).

---

# Legend

**Priority**
- P0 — Critical Foundation
- P1 — Core
- P2 — Important
- P3 — Supporting

**Difficulty**
- 🟢 Foundation
- 🟡 Intermediate
- 🔴 Advanced

**Status**
- `[ ]` Not Started
- `[/]` In Progress
- `[x]` Completed

---

# Stage 0 — Engineering Foundations Repository

| ID | Module | Priority | Difficulty | Duration | Status |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **P-01** | Git & Version Control Internals | P0 | 🟢 | 2 Days | `[x]` |
| **P-02** | Linux System Foundations | P0 | 🟢 | 1 Week | `[x]` |
| **P-03** | Windows System Foundations | P0 | 🟢 | 1 Week | `[x]` |
| **P-04** | Python Systems Programming | P0 | 🟢 | Ongoing | `[x]` |
| **P-05** | Relational Databases & SQL | P0 | 🟢 | 3 Days | `[x]` |
| **P-06** | HTTP & REST Architecture | P0 | 🟢 | 3 Days | `[x]` |
| **P-07** | Process Architecture & Lifecycle | P0 | 🟢 | 2 Days | `[x]` |
| **P-08** | Threading & Concurrency | P1 | 🟢 | 1 Day | `[x]` |
| **P-09** | Virtual Memory & Allocation | P0 | 🟡 | 3 Days | `[x]` |
| **P-10** | Filesystems & VFS Internals | P0 | 🟢 | 2 Days | `[x]` |
| **P-11** | Network Protocol Stack | P0 | 🟡 | 1 Week | `[x]` |
| **P-12** | Authentication Internals | P0 | 🟡 | 3 Days | `[x]` |
| **P-13** | Logging Fundamentals & Telemetry | P0 | 🟢 | 2 Days | `[x]` |
| **P-14** | Linux Kernel Architecture & Syscalls | P1 | 🟡 | 4 Days | `[x]` |
| **P-15** | Windows Architecture & Security Model | P1 | 🟡 | 4 Days | `[x]` |

---

# Stage 2 — Secure Software Engineering

| ID | Module | Priority | Duration | Status |
| :--- | :--- | :--- | :--- | :---: |
| **S2-M01** | Authentication Mechanisms | P0 | 1 Week | `[ ]` |
| **S2-M02** | Authorization Systems | P0 | 1 Week | `[ ]` |
| **S2-M03** | Sessions & Cookies | P0 | 3 Days | `[ ]` |
| **S2-M04** | JWT Deep Dive | P1 | 3 Days | `[ ]` |
| **S2-M05** | OAuth2 / OIDC | P1 | 1 Week | `[ ]` |
| **S2-M06** | Multi-Factor Authentication | P1 | 3 Days | `[ ]` |
| **S2-M07** | Secure API Architecture | P0 | 1 Week | `[ ]` |
| **S2-M08** | Input Validation | P0 | 3 Days | `[ ]` |
| **S2-M09** | CSRF Defenses | P1 | 2 Days | `[ ]` |
| **S2-M10** | XSS Prevention | P1 | 2 Days | `[ ]` |
| **S2-M11** | SQL Injection Mitigations | P0 | 2 Days | `[ ]` |
| **S2-M12** | Secure File Upload Systems | P1 | 3 Days | `[ ]` |
| **S2-M13** | Applied Cryptography | P0 | 1 Week | `[ ]` |
| **S2-M14** | Secrets Management | P1 | 2 Days | `[ ]` |
| **S2-M15** | Audit Logging Systems | P0 | 4 Days | `[ ]` |
| **S2-M16** | Threat Modeling | P0 | 4 Days | `[ ]` |
| **S2-M17** | Security Testing | P0 | 1 Week | `[ ]` |
| **S2-M18** | OWASP Top 10 Review | P1 | 3 Days | `[ ]` |

---

# Stage 3 — Attack Lifecycle

| ID | Module | Priority | Status |
| :--- | :--- | :--- | :---: |
| **S3-M01** | Reconnaissance | P0 | `[ ]` |
| **S3-M02** | Initial Access | P0 | `[ ]` |
| **S3-M03** | Execution | P0 | `[ ]` |
| **S3-M04** | Persistence | P0 | `[ ]` |
| **S3-M05** | Privilege Escalation | P0 | `[ ]` |
| **S3-M06** | Defense Evasion | P0 | `[ ]` |
| **S3-M07** | Credential Access | P0 | `[ ]` |
| **S3-M08** | Discovery | P0 | `[ ]` |
| **S3-M09** | Lateral Movement | P0 | `[ ]` |
| **S3-M10** | Collection | P1 | `[ ]` |
| **S3-M11** | Exfiltration | P1 | `[ ]` |
| **S3-M12** | Command & Control | P1 | `[ ]` |
| **S3-M13** | Impact | P1 | `[ ]` |

---

# Stage 4 — Detection Engineering

| ID | Module | Priority | Status |
| :--- | :--- | :--- | :---: |
| **S4-M01** | Windows Event Logs | P0 | `[ ]` |
| **S4-M02** | Sysmon Telemetry | P0 | `[ ]` |
| **S4-M03** | Linux Auditd & Logs | P1 | `[ ]` |
| **S4-M04** | Event Tracing for Windows (ETW) | P2 | `[ ]` |
| **S4-M05** | Sigma Rule Engineering | P0 | `[ ]` |
| **S4-M06** | KQL Queries | P2 | `[ ]` |
| **S4-M07** | SPL Queries | P2 | `[ ]` |
| **S4-M08** | Log Normalization | P0 | `[ ]` |
| **S4-M09** | Event Correlation | P0 | `[ ]` |
| **S4-M10** | IOC Matching Engines | P1 | `[ ]` |
| **S4-M11** | MITRE ATT&CK Mapping | P0 | `[ ]` |
| **S4-M12** | Threat Hunting | P1 | `[ ]` |
| **S4-M13** | Incident Response Workflows | P1 | `[ ]` |
| **S4-M14** | Detection Validation | P1 | `[ ]` |

---

# Progress Summary

| Stage | Completed | Total | Percentage |
| :--- | :---: | :---: | :---: |
| **Stage 0: Foundations** | **15** | **15** | **100%** |
| **Stage 2: Secure Software** | 0 | 18 | 0% |
| **Stage 3: Attack Lifecycle** | 0 | 13 | 0% |
| **Stage 4: Detection Engineering** | 0 | 14 | 0% |
| **Total** | **15** | **60** | **25%** |
