---
id: "mod-win-tokens-privs"
title: "Windows Access Tokens & Privilege Security"
domain: "Domain-01"
branch: "windows-internals"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Windows Access Tokens & Privilege Security

## 1. Overview & Purpose
In Windows, access control and security context are encapsulated within kernel **Access Token** objects (`TOKEN`). Every process and thread executing privileged operations does so under the authority of an active primary or impersonation token.

This module covers the internal structure of `TOKEN`, Primary vs Impersonation tokens, Token Privileges (`SeDebugPrivilege`, `SeImpersonatePrivilege`, `SeAssignPrimaryTokenPrivilege`), User Account Control (UAC) restricted tokens, and token manipulation attack primitives (Token Theft, Token Impersonation, Parent PID Spoofing).

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Kernel Space (Ring 0)
        EPROC["EPROCESS Structure"]
        ETHREAD["ETHREAD Structure"]
        
        PTOKEN["Primary Access Token<br/>(EPROCESS -> Token: _EX_FAST_REF)"]
        ITOKEN["Impersonation Access Token<br/>(ETHREAD -> ActiveImpersonationToken)"]

        EPROC --> PTOKEN
        ETHREAD -.->|Optional Thread Impersonation| ITOKEN
    end

    subgraph TOKEN Object Kernel Structure
        TOKEN_OBJ["_TOKEN Structure<br/>├── TokenId / AuthenticationId (LUID)<br/>├── UserAndGroupCount & UserAndGroups (SIDs)<br/>├── Privileges (Enabled / Present / Default Bitmasks)<br/>├── TokenType (1 = Primary, 2 = Impersonation)<br/>└── ImpersonationLevel (Anonymous, Identification, Impersonation, Delegation)"]
    end

    PTOKEN --> TOKEN_OBJ
    ITOKEN --> TOKEN_OBJ
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 Primary Tokens vs Impersonation Tokens
- **Primary Token**: Created by Executive during process initialization (`NtCreateUserProcess`). Represents the default security context for all threads in the process.
- **Impersonation Token**: Assigned to a specific `ETHREAD` (`NtImpersonateThread`). Allows a thread to temporarily execute under a different security context (e.g., a service handling a client RPC request).

#### Impersonation Levels:
1. `SecurityAnonymous`: Cannot obtain identification or impersonate.
2. `SecurityIdentification`: Can obtain identity (SID) but cannot impersonate.
3. `SecurityImpersonation`: Can impersonate client on local system (**Required for local privilege escalation**).
4. `SecurityDelegation`: Can impersonate client across network systems (Requires Kerberos Unconstrained/Constrained Delegation).

---

### 3.2 Token Privileges Structure
Privileges represent rights to perform specific system-wide operations (e.g., debugging processes, loading drivers, shutting down system).

Inside `_TOKEN`, privileges are represented as LUID (Locally Unique Identifier) bitfields:
- `Privileges.Present`: Bitmask of privileges held by the account.
- `Privileges.Enabled`: Bitmask of privileges currently active.
- `Privileges.EnabledByDefault`: Privileges enabled when token was generated.

#### Key Security Privileges:
- `SeDebugPrivilege` (`20`): Allows opening any process with `PROCESS_ALL_ACCESS` (including `lsass.exe`).
- `SeImpersonatePrivilege` (`29`): Allows impersonating any client token (Target for Potato exploits: JuicyPotato, RoguePotato, SweetPotato).
- `SeAssignPrimaryTokenPrivilege` (`3`): Allows assigning primary tokens to child processes.
- `SeLoadDriverPrivilege` (`10`): Allows loading kernel-mode drivers (BYOVD attack primitive).

---

### 3.3 User Account Control (UAC) & Restricted Tokens
When an Administrator logs into a system with UAC enabled, LSA generates **two tokens**:
1. **Filtered / Restricted Token**: Default token assigned to `explorer.exe`. Strips administrative groups (`S-1-5-32-544`) and disables elevated privileges.
2. **Elevated Token**: Full token stored in memory, granted only when the user accepts a UAC prompt (`consent.exe` / `elevate.exe`).

---

## 4. Security Implications & Access Control Checks

When a thread attempts to open an object handle (e.g., `OpenProcess` on `lsass.exe`), the kernel Executive performs an access check (`SeAccessCheck`):
1. Selects active thread token (Impersonation token if present, else Primary token).
2. Extracts Security Descriptor (DACL) from target object.
3. Compares Token SIDs and Enabled Privileges against DACL Ace entries.
4. If a matching Access Allowed ACE is found matching requested access rights, access is granted.

---

## 5. Attack Vectors & Exploitation Primitives

1. **Token Impersonation & Theft (`OpenProcessToken` + `DuplicateTokenEx`)**:
   Opening a handle to a process running as `NT AUTHORITY\SYSTEM` (e.g., `winlogon.exe`), duplicating its token, and spawning a cmd shell (`CreateProcessWithTokenW`).
2. **Potato Exploits (`SeImpersonatePrivilege` Abuse)**:
   Triggering a local DCOM/RPC service running as `NT AUTHORITY\SYSTEM` to connect to a attacker-controlled rogue pipe/RPC server, stealing the incoming `SYSTEM` token, and impersonating it via `SetThreadToken`.
3. **UAC Bypass**:
   Abusing auto-elevating COM objects (`IFileOperation`) or DLL hijacking in protected directories (`C:\Windows\System32`) to execute code using the Elevated Token without triggering a user prompt.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **Windows Security Event ID 4672**: Special privileges assigned to new logon (captures `SeDebugPrivilege`, `SeImpersonatePrivilege`).
- **Windows Security Event ID 4673**: Sensitive privilege use (`SeDebugPrivilege` invoked).
- **Sysmon Event ID 10**: ProcessAccess (captures attempts to open handles to privileged processes).

### Sigma Detection Rule Snippet (SeDebugPrivilege Enablement & Access):
```yaml
title: SeDebugPrivilege Enabled Process Access to Sensitive Targets
id: b91029e1-12a4-47b2-a091-827101fa91bc
status: experimental
description: Detects processes leveraging SeDebugPrivilege to open high-privilege targets like LSASS.
logsource:
  category: process_access
  product: windows
detection:
  selection:
    TargetImage|endswith: '\lsass.exe'
    GrantedAccess|contains: '0x1400' # PROCESS_VM_READ | PROCESS_QUERY_INFORMATION
  condition: selection
level: high
```

---

## 7. Engineering & Hands-On Implementation

### Inspecting Process Tokens in WinDbg:
```text
kd> !token ffffe001ab551060
User: S-1-5-18 (NT AUTHORITY\SYSTEM)
Groups: 
 00 S-1-5-32-544 (Builtin\Administrators) Enabled
 01 S-1-5-11 (NT AUTHORITY\Authenticated Users) Enabled

Privileges:
 03 SeAssignPrimaryTokenPrivilege  Attributes - Enabled
 10 SeLoadDriverPrivilege          Attributes - Enabled
 20 SeDebugPrivilege               Attributes - Enabled
 29 SeImpersonatePrivilege          Attributes - Enabled
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| `OpenProcess()` returns `ERROR_ACCESS_DENIED` (`5`). | Process token lacks `SeDebugPrivilege` or requested access mask exceeds DACL permissions. | Enable `SeDebugPrivilege` via `AdjustTokenPrivileges()` API before invoking call. |
| Potato exploit fails on Server 2019/2022. | Service account lacks `SeImpersonatePrivilege` or RPC port 135 blocking local redirection. | Verify privileges via `whoami /priv`. Use PrintSpoofer / RoguePotato for non-DCOM pipes. |

---

## 9. References
- Mark Russinovich, Pavel Yosifovich, Alex Ionescu, *Windows Internals, Part 1 (7th Edition)*.
- James Forshaw (Google Project Zero), *Access Control Decisions in Windows Kernel*.
