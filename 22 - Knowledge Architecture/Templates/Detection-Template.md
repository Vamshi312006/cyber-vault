---
id: "DET-XXXXXX"
title: "Detection: [Detection Title]"
domain: "DOM-XX"
branch: "BR-XX.YY"
type: "detection"
mitre_attack_id: "T1055.001"
severity: "High"
maintainer: "Cyber Act Detection Engineering Team"
last_audited: "YYYY-MM-DD"
---

# Detection Engineering Specification: [Detection Title]

## 1. Threat Scenario & Attack Primitive
- **Detection ID**: `DET-XXXXXX`
- **MITRE ATT&CK Mapping**: `T1055.001` (Process Injection: Dynamic-link Library Injection)
- **Target Component**: `MOD-XX.YY.ZZ`

[Description of the adversary behavior, technique, and telemetry footprint.]

---

## 2. Telemetry Requirements & Log Sources

| Data Provider | Event ID / Channel | Critical Fields Required |
| :--- | :--- | :--- |
| **Sysmon** | Event ID 8 (CreateRemoteThread) | `SourceImage`, `TargetImage`, `StartAddress` |
| **Windows Security** | Event ID 4688 | `ProcessId`, `ParentProcessId`, `CommandLine` |

---

## 3. Production Detection Analytics

### 3.1 Sigma Rule
```yaml
title: [Detection Title]
id: [UUID]
status: experimental
description: [Detection Description]
references:
  - https://attack.mitre.org/techniques/T1055/
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    ParentImage|endswith: '\lsass.exe'
  condition: selection
falsepositives:
  - Legitimate system error reporting
level: high
```

### 3.2 KQL Query (Microsoft Sentinel)
```kql
SecurityEvent
| where EventID == 4688
| where ParentProcessName endswith "lsass.exe"
```

---

## 4. False Positive Analysis & Tuning Guidelines
- **False Positive Source 1**: [Legitimate process trigger].
- **Tuning Strategy**: [Exclusion rule or hash baseline].

---

## 5. Testing & Validation Verification
- Execute `LAB-XXXXXX` to generate real telemetry.
- Verify rule triggers in SIEM instance.
