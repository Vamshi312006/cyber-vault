---
id: "DOM-07"
title: "Domain 07: Endpoint Security - Master Knowledge Architecture"
type: "domain-specification"
domain_id: "07"
maintainer: "Cyber Act Endpoint Security Team"
last_audited: "2026-07-29"
---

# Domain 07: Endpoint Security — Master Knowledge Architecture

## 1. Domain Identity & Overview
- **Domain ID**: `DOM-07`
- **Canonical Name**: Endpoint Security
- **Ontology Parent**: `KU-CYBER`
- **Domain Status**: `Active`

`DOM-07` encompasses the engineering principles, operating system controls, EDR agent telemetry mechanisms, process protection models, application whitelisting, and host security strategies required to defend host computing environments (workstations, servers, container hosts).

---

## 2. Scope & Exclusion Boundaries
- **In Scope**: EDR agent architectures, Sysmon, Event Tracing for Windows (ETW), Linux `auditd`, AppLocker / WDAC policies, SELinux / AppArmor profiles, LSASS memory protections, Credential Guard, and Host Intrusion Prevention Systems (HIPS).
- **Explicit Exclusions**:
  - OS Kernel internal process scheduling & syscall table mechanics (governed by `DOM-01`).
  - Network perimeter firewalls & packet routing (governed by `DOM-02`).
  - Software secure coding and application vulnerabilities (governed by `DOM-04`).

---

## 3. Major Engineering Branches
- `edr-agent-architecture`
- `host-telemetry-etw-auditd`
- `application-whitelisting-wdac`
- `host-isolation-lsass-protection`
- `endpoint-hardening-baselines`

---

## 4. Dependencies
- `DOM-01` (Systems & Kernel Security)
