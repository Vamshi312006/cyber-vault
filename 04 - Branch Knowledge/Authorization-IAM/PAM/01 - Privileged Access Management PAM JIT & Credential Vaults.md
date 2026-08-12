---
id: "MOD-06.04.01"
title: "Privileged Access Management (PAM), Just-In-Time (JIT) Access & Break-Glass Vault Architecture"
domain: "DOM-06"
branch: "BR-06.04"
type: "module"
maintainer: "Cyber Act IAM & Authorization Security Team"
last_audited: "2026-07-29"
---

# Privileged Access Management (PAM), Just-In-Time (JIT) Access & Break-Glass Vault Architecture

## 1. Overview & Purpose
Privileged accounts (Domain Admins, Root, Cloud Global Administrators) possess elevated permissions that represent high-impact targets for cyber adversaries.

This module details Privileged Access Management (PAM) Architecture, Just-In-Time (JIT) Elevation, Just-Enough-Administration (JEA), Secret/Credential Vaulting (HashiCorp Vault / CyberArk), Privileged Session Monitoring, and Emergency Break-Glass Vault Procedures.

---

## 2. Prerequisites & Prerequisites Graph
- **Prerequisites**: `MOD-06.02.01` (RBAC Models) & `MOD-05.04.01` (Credential Security).

```mermaid
graph TD
    PRE1["MOD-06.02.01<br/>(RBAC Models)"] --> THIS["MOD-06.04.01<br/>(PAM, JIT & Break-Glass Vaults)"]
    PRE2["MOD-05.04.01<br/>(Credential Security)"] --> THIS
```

---

## 3. Learning Objectives & Taxonomy Alignment
- **L1 Awareness**: Contrast Standing Privileges and Just-In-Time (JIT) Privileged Access.
- **L2 Understanding**: Detail PAM vault credential check-in/check-out auto-rotation, Shamir's Secret Sharing break-glass mechanics, and JEA PowerShell constrained endpoints.
- **L3 Practical**: Programmatically request temporary Vault credentials using HashiCorp Vault API in Python.
- **L4 Engineering**: Design zero-standing-privilege enterprise architectures eliminating permanent administrative accounts.

---

## 4. L1 — Awareness (Overview & Core Terminology)
**Standing Privileges** leave administrative accounts permanently enabled, exposing them to credential theft 24/7. **PAM (Privileged Access Management)** enforces **Just-In-Time (JIT)** access: administrators possess zero standing privileges and must request temporary, time-bound access (e.g. 1 hour) approved by ticket workflows.

---

## 5. L2 — Understanding (Core Theory & Mechanics)

```mermaid
graph TD
    subgraph Zero-Standing-Privilege JIT Access Workflow
        ADMIN["System Administrator"]
        PAM_VAULT["PAM Vault / Approval Engine (CyberArk / Teleport / HashiCorp Vault)"]
        TARGET_HOST["Production Server (Linux Root / Windows DC)"]

        ADMIN -->|1. Requests JIT Access (Reason: Incident INC-9942, TTL: 60 mins)| PAM_VAULT
        PAM_VAULT <-->|2. Validates Ticket Approval & MFA Challenge| TICKETING["Jira / ServiceNow API"]

        PAM_VAULT -->|3. Dynamically Provisions Temporary SSH Certificate / Rotated Password| ADMIN
        ADMIN -->|4. Authenticates via Encrypted PAM Proxy Session| TARGET_HOST

        PAM_VAULT -->|5. Session Monitor Records Screen / Keystrokes| AUDIT["SIEM Audit Log"]
        PAM_VAULT -->|6. TTL Expires (60m) -> Automatically Revokes Access & Rotates Secret!| TARGET_HOST
    end
```

### Break-Glass Emergency Account Architecture:
Emergency Break-Glass accounts are used *only* during catastrophic outages (e.g. Identity Provider down). Break-glass credentials are split into multiple parts using **Shamir's Secret Sharing ($k$ of $n$ threshold)** and stored in sealed physical/digital vaults. Accessing a break-glass account triggers critical severity alerts to security operations.

---

## 6. L3 — Practical (Commands & Configurations)

### Generating Dynamic Temporary AWS Credentials via HashiCorp Vault API in Python:
```python
import hvac

# Connect to HashiCorp Vault Server
client = hvac.Client(url="https://vault.corp.internal:8200", token="hvs.CAES...")

def get_jit_aws_credentials(role_name: str) -> dict:
    # Request dynamic temporary AWS credentials (Vault generates 1-hour IAM keys)
    response = client.secrets.aws.generate_credentials(
        name=role_name,
        role_arn="arn:aws:iam::123456789012:role/DatabaseAdminRole"
    )

    creds = response["data"]
    print(f"Temporary Access Key: {creds['access_key']}")
    print(f"Lease Duration: {response['lease_duration']} seconds")
    return creds

# Generates 1-hour temporary credentials -> Automatically expires on Vault server!
```

---

## 7. L4 — Engineering (Design Trade-offs & System Integration)
- **Zero Standing Privileges vs Incident Response Latency Trade-off**: Enforcing JIT access for every administrative action introduces 2–5 minutes of approval latency. To mitigate this during critical outages, PAM engines must support automated pre-approved emergency break-glass workflows with mandatory post-incident audit recertification.

---

## 8. Internal Architecture & Data Structures
HashiCorp Vault Dynamic Secret Lease Lease Object Format:
```json
{
  "request_id": "88410294-8210-41ab...",
  "lease_id": "aws/creds/db-admin/h391029...",
  "renewable": true,
  "lease_duration": 3600,
  "data": {
    "access_key": "AKIAIOSFODNN7EXAMPLE",
    "secret_key": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
  }
}
```

---

## 9. Security Implications & Boundary Controls
- **Never Leave Static Root / Admin Passwords**: Leaving un-rotated static root passwords on servers bypasses PAM controls, enabling attackers to gain permanent persistence.

---

## 10. Attack Vectors & Exploitation Primitives
1. **Standing Privileged Account Compromise**: Dumping domain admin hashes from machines where admins remain logged in indefinitely.
2. **Break-Glass Vault Abuse**: Unlawfully checking out break-glass credentials without authorization during off-hours.

---

## 11. Defense & Telemetry Verification
- Eliminate **Standing Privileges** in favor of **Just-In-Time (JIT)** dynamic access.
- Deploy **Keystroke & Session Recording Proxies** for all SSH/RDP administrative sessions.

---

## 12. Detection & Telemetry Verification

### Telemetry Alert for Break-Glass Account Usage:
```yaml
title: Emergency Break-Glass Account Activated
id: a9102941-8210-41ab-b01b-920191fa6405
logsource:
  category: authentication
  product: active_directory
detection:
  selection:
    TargetUserName: "bg_admin_emergency"
    EventID: 4624
  condition: selection
level: critical
```

---

## 13. Hands-On Lab Reference
- **Associated Lab**: `LAB-AUT004` (HashiCorp Vault JIT Credential Provisioning & Break-Glass Setup).

---

## 14. Related Projects & Applications
- **Associated Project**: `PRJ-SEC001` (Secure Healthcare Platform).

---

## 15. Troubleshooting & Diagnostics Matrix

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| `VaultException: lease expired`. | Temporary JIT lease exceeded time-to-live (TTL) limit. | Issue `vault lease renew <lease_id>` or request new dynamic credentials. |

---

## 16. Related Concepts & Cross-Domain Links
- `CON-AUT007`: Just-In-Time JIT Privileged Access (`DOM-06`)
- `CON-AUT008`: Break-Glass Vault Architecture (`DOM-06`)
- `CON-SEC004`: HashiCorp Vault Secret Management (`DOM-04`)

---

## 17. Technical Interview Preparation (Q&A)
**Q: What is "Zero Standing Privileges" (ZSP) and how does a modern PAM system implement Just-In-Time (JIT) access?**  
*Answer*: Zero Standing Privileges (ZSP) is an IAM engineering paradigm where no user account possesses permanent administrative privileges on systems. In a traditional setup, administrators hold permanent `Domain Admin` or `root` credentials exposed to 24/7 theft. In a JIT-enabled PAM architecture, administrative accounts remain unprivileged or disabled by default. When an admin requires access to resolve a ticket, they request time-bound access through a PAM vault (e.g. Teleport / CyberArk / Vault). The PAM engine validates ticket approval, issues a temporary, short-lived (e.g. 1-hour) SSH certificate or dynamically generated cloud key, logs all session keystrokes, and automatically revokes access when the lease expires.

---

## 18. Self-Assessment & Mastery Checklist
- [ ] Understand Standing Privileges vs JIT access models.
- [ ] Able to write Python code interacting with HashiCorp Vault dynamic AWS/Database secrets engines.

---

## 19. References & Further Reading
- NIST SP 800-207: *Zero Trust Architecture (Section 3.1 Privileged Access Management)*.
- HashiCorp Vault Documentation: *Dynamic Secrets Architecture*.
