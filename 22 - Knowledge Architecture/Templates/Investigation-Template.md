---
id: "INV-XXXXXX"
title: "Investigation: [Investigation Title]"
domain: "DOM-XX"
branch: "BR-XX.YY"
type: "investigation"
incident_type: "Ransomware / Intrusion"
maintainer: "Cyber Act Incident Response Team"
last_audited: "YYYY-MM-DD"
---

# Threat Investigation Playbook: [Investigation Title]

## 1. Incident Context & Triage Criteria
- **Investigation ID**: `INV-XXXXXX`
- **Initial Trigger**: Alert `DET-XXXXXX` fired on endpoint `[Hostname]`.

[Brief executive scenario describing the incident indicator.]

---

## 2. Investigation Workflow & Triage Tree

```mermaid
graph TD
    ALERT["Alert Triggered: DET-XXXXXX"] --> Q1{"Is Parent Process Suspicious?"}
    Q1 -- Yes --> MEM["Dump Memory via WinDbg / Volatility"]
    Q1 -- No --> FP["Close as False Positive"]
    MEM --> NET["Inspect Outbound C2 Sockets"]
```

---

## 3. Evidence Artifact Collection Checklist

| Artifact Source | Location / Command | Investigation Purpose |
| :--- | :--- | :--- |
| **Process Tree** | `tasklist /v` or Sysmon Event 1 | Identify child process lineage |
| **Network Sockets** | `netstat -ano` or `ss -tulpn` | Identify active C2 connections |
| **Memory Dump** | `winpmem` / `dumpit` | Extract un-encrypted payload |

---

## 4. Forensics Query Playbook
```kql
// Sentinel Forensic Query for Host Timeline
DeviceProcessEvents
| where DeviceName == "HOST-01"
| where Timestamp >= ago(24h)
| order by Timestamp asc
```

---

## 5. Containment & Remediation Actions
1. **Network Isolation**: Isolate host from LAN via EDR command.
2. **Credential Revocation**: Reset active User Account Access Tokens.
3. **IOC Scrubber**: Sweep enterprise SIEM for matching IP / MD5 hashes.
