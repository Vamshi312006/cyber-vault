# Cyber Act Module Index

> Status: Active
> Version: 1.0
> Last Updated:

---

# Purpose

This document is the master index of every Cyber Act module.

It provides a single location to track module IDs, priorities, dependencies, duration, implementation status, and project mappings.

Detailed content belongs in the individual module notes.

---

# Legend

Priority

- P0 — Critical Foundation
- P1 — Core
- P2 — Important
- P3 — Supporting

Difficulty

- 🟢 Foundation
- 🟡 Intermediate
- 🔴 Advanced

Status

- [ ] Not Started
- [~] In Progress
- [x] Completed

---

# Stage 0 — Engineering Foundations Repository

| ID | Module | Priority | Difficulty | Duration | Status |
|----|--------|----------|------------|----------|--------|
| P-01 | Git | P0 | 🟢 | 2 Days | [ ] |
| P-02 | Linux | P0 | 🟢 | 1 Week | [ ] |
| P-03 | Windows | P0 | 🟢 | 1 Week | [ ] |
| P-04 | Python | P0 | 🟢 | Ongoing | [ ] |
| P-05 | SQL | P0 | 🟢 | 3 Days | [ ] |
| P-06 | HTTP / REST | P0 | 🟢 | 3 Days | [ ] |
| P-07 | Processes | P0 | 🟢 | 2 Days | [ ] |
| P-08 | Threads | P1 | 🟢 | 1 Day | [ ] |
| P-09 | Memory | P0 | 🟡 | 3 Days | [ ] |
| P-10 | Filesystems | P0 | 🟢 | 2 Days | [ ] |
| P-11 | Networking Internals | P0 | 🟡 | 1 Week | [ ] |
| P-12 | Authentication Internals | P0 | 🟡 | 3 Days | [ ] |
| P-13 | Logging Fundamentals | P0 | 🟢 | 2 Days | [ ] |
| P-14 | Linux Internals | P1 | 🟡 | 4 Days | [ ] |
| P-15 | Windows Internals | P1 | 🟡 | 4 Days | [ ] |

---

# Stage 2 — Secure Software Engineering

| ID | Module | Priority | Duration | HC V2 | Status |
|----|--------|----------|----------|-------|--------|
| S2-M01 | Authentication | P0 | 1 Week | ✓ | [ ] |
| S2-M02 | Authorization | P0 | 1 Week | ✓ | [ ] |
| S2-M03 | Sessions & Cookies | P0 | 3 Days | ✓ | [ ] |
| S2-M04 | JWT | P1 | 3 Days | ✓ | [ ] |
| S2-M05 | OAuth2 / OIDC | P1 | 1 Week | ✓ | [ ] |
| S2-M06 | MFA | P1 | 3 Days | ✓ | [ ] |
| S2-M07 | Secure APIs | P0 | 1 Week | ✓ | [ ] |
| S2-M08 | Validation | P0 | 3 Days | ✓ | [ ] |
| S2-M09 | CSRF | P1 | 2 Days | ✓ | [ ] |
| S2-M10 | XSS | P1 | 2 Days | ✓ | [ ] |
| S2-M11 | SQL Injection | P0 | 2 Days | ✓ | [ ] |
| S2-M12 | Secure File Upload | P1 | 3 Days | ✓ | [ ] |
| S2-M13 | Cryptography | P0 | 1 Week | ✓ | [ ] |
| S2-M14 | Secrets Management | P1 | 2 Days | ✓ | [ ] |
| S2-M15 | Audit Logging | P0 | 4 Days | ✓ | [ ] |
| S2-M16 | Threat Modeling | P0 | 4 Days | ✓ | [ ] |
| S2-M17 | Security Testing | P0 | 1 Week | ✓ | [ ] |
| S2-M18 | OWASP Review | P1 | 3 Days | ✓ | [ ] |

---

# Stage 3 — Attack Lifecycle

| ID | Module | Priority | HTB | DE | Status |
|----|--------|----------|-----|----|--------|
| S3-M01 | Reconnaissance | P0 | ✓ | ✓ | [ ] |
| S3-M02 | Initial Access | P0 | ✓ | ✓ | [ ] |
| S3-M03 | Execution | P0 | ✓ | ✓ | [ ] |
| S3-M04 | Persistence | P0 | ✓ | ✓ | [ ] |
| S3-M05 | Privilege Escalation | P0 | ✓ | ✓ | [ ] |
| S3-M06 | Defense Evasion | P0 | ✓ | ✓ | [ ] |
| S3-M07 | Credential Access | P0 | ✓ | ✓ | [ ] |
| S3-M08 | Discovery | P0 | ✓ | ✓ | [ ] |
| S3-M09 | Lateral Movement | P0 | ✓ | ✓ | [ ] |
| S3-M10 | Collection | P1 | ✓ | ✓ | [ ] |
| S3-M11 | Exfiltration | P1 | ✓ | ✓ | [ ] |
| S3-M12 | Command & Control | P1 | ✓ | ✓ | [ ] |
| S3-M13 | Impact | P1 | ✓ | ✓ | [ ] |

---

# Stage 4 — Detection Engineering

| ID | Module | Priority | DE Platform | Status |
|----|--------|----------|-------------|--------|
| S4-M01 | Windows Event Logs | P0 | ✓ | [ ] |
| S4-M02 | Sysmon | P0 | ✓ | [ ] |
| S4-M03 | Linux Logs | P1 | ✓ | [ ] |
| S4-M04 | ETW | P2 | ✓ | [ ] |
| S4-M05 | Sigma | P0 | ✓ | [ ] |
| S4-M06 | KQL | P2 | Optional | [ ] |
| S4-M07 | SPL | P2 | Optional | [ ] |
| S4-M08 | Log Normalization | P0 | ✓ | [ ] |
| S4-M09 | Correlation | P0 | ✓ | [ ] |
| S4-M10 | IOC Matching | P1 | ✓ | [ ] |
| S4-M11 | ATT&CK Mapping | P0 | ✓ | [ ] |
| S4-M12 | Threat Hunting | P1 | ✓ | [ ] |
| S4-M13 | Incident Response | P1 | ✓ | [ ] |
| S4-M14 | Detection Validation | P1 | ✓ | [ ] |

---

# Stage 5 — Enterprise Security

*(See Curriculum for module list.)*

---

# Stage 6 — Professional Engineering

*(See Curriculum for module list.)*

---

# Progress Summary

| Stage | Completed | Total |
|--------|----------:|------:|
| Foundations | 0 | 15 |
| Secure Software | 0 | 18 |
| Attack Lifecycle | 0 | 13 |
| Detection Engineering | 0 | 14 |
| Enterprise | 0 | 9 |
| Professional | 0 | 11 |
