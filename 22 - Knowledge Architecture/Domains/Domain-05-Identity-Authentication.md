---
id: "DOM-05"
title: "Domain 05: Identity & Authentication - Master Knowledge Architecture"
type: "domain-specification"
domain_id: "05"
maintainer: "Cyber Act Identity Engineering Team"
last_audited: "2026-07-29"
---

# Domain 05: Identity & Authentication — Master Knowledge Architecture

## 1. Domain Identity & Overview
- **Domain ID**: `DOM-05`
- **Canonical Name**: Identity & Authentication
- **Ontology Parent**: `KU-CYBER`
- **Domain Status**: `Active`

`DOM-05` establishes the foundational engineering principles, cryptographic protocols, token verification mechanisms, enterprise authentication models, and credential protection architectures required to prove and verify digital identity securely across human and machine actors.

---

## 2. Scope & Exclusion Boundaries
- **In Scope**: Digital identity lifecycle, human vs machine identity, password hashing (Argon2id/bcrypt), MFA (TOTP/FIDO2 WebAuthn), enterprise auth protocols (Kerberos, NTLM, LDAP), web federation (SAML 2.0, OAuth 2.0, OpenID Connect - OIDC), JWT token security, credential attack vectors (Pass-the-Hash, Pass-the-Ticket, Golden Ticket, Password Spraying), and workload identity (SPIFFE/SPIRE, Kubernetes ServiceAccounts).
- **Explicit Exclusions**:
  - Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), and Authorization Policies (governed by `DOM-06`).
  - Active Directory Domain Services administration & LDAP directory schemas (governed by `DOM-10`).
  - Cryptographic key exchange primitives and PKI CA operations (governed by `DOM-03`).

---

## 3. Branch Decomposition Matrix

Domain 05 is partitioned into **7 Core Engineering Branches**:

```
Domain-05: Identity & Authentication
├── Branch 05.1: Digital Identity Foundations (identity-foundations)
├── Branch 05.2: Authentication Mechanisms (auth-mechanisms)
├── Branch 05.3: Authentication Protocols (auth-protocols)
├── Branch 05.4: Credential Security (credential-security)
├── Branch 05.5: Federation & Single Sign-On (federation-sso)
├── Branch 05.6: Identity Attacks & Defenses (identity-attacks-defenses)
└── Branch 05.7: Modern Identity Engineering (modern-identity-engineering)
```

---

## 4. Branch & Module Breakdown

### Branch 05.1: Digital Identity Foundations (`identity-foundations`)
- **`MOD-05.01.01`**: Digital Identity Lifecycle, Identity Stores & Human vs Machine Identity Architecture

### Branch 05.2: Authentication Mechanisms (`auth-mechanisms`)
- **`MOD-05.02.01`**: Passwordless Authentication, MFA (TOTP, FIDO2 / WebAuthn) & Hardware Passkeys

### Branch 05.3: Authentication Protocols (`auth-protocols`)
- **`MOD-05.03.01`**: Enterprise Authentication Protocols (Kerberos v5, NTLMv2 & LDAP)
- **`MOD-05.03.02`**: Web & API Authentication Protocols (SAML 2.0, OAuth 2.0 & OpenID Connect - OIDC)

### Branch 05.4: Credential Security (`credential-security`)
- **`MOD-05.04.01`**: Password Hashing Algorithms (Argon2id, bcrypt, PBKDF2) & In-Memory Credential Dumping

### Branch 05.5: Federation & Single Sign-On (`federation-sso`)
- **`MOD-05.05.01`**: Identity Federation, Single Sign-On (SSO) Architecture & JWT Token Validation

### Branch 05.6: Identity Attacks & Defenses (`identity-attacks-defenses`)
- **`MOD-05.06.01`**: Identity Attack Primitives (Credential Stuffing, Pass-the-Hash, Pass-the-Ticket & Golden Tickets)

### Branch 05.7: Modern Identity Engineering (`modern-identity-engineering`)
- **`MOD-05.07.01`**: Cloud & Workload Identity Engineering (SPIFFE/SPIRE, Kubernetes ServiceAccounts & Cloud IAM Roles)

---

## 5. Module Dependency Graph (Mermaid DAG)

```mermaid
graph TD
    subgraph Layer 0: Identity & Credential Foundations
        M_IDF["MOD-05.01.01<br/>(Digital Identity Foundations)"]
        M_HASH["MOD-05.04.01<br/>(Password Hashing Argon2id & Dumping)"]
    end

    subgraph Layer 1: Authentication Mechanisms & Enterprise Protocols
        M_MFA["MOD-05.02.01<br/>(MFA, FIDO2 & WebAuthn)"]
        M_KERB["MOD-05.03.01<br/>(Kerberos v5 & NTLMv2)"]
    end

    subgraph Layer 2: Web Federation & Single Sign-On
        M_OAUTH["MOD-05.03.02<br/>(SAML 2.0, OAuth 2.0 & OIDC)"]
        M_SSO["MOD-05.05.01<br/>(Identity Federation & JWT Tokens)"]
    end

    subgraph Layer 3: Attacks, Defenses & Machine Identity
        M_ATT["MOD-05.06.01<br/>(Identity Attacks & Golden Tickets)"]
        M_WORK["MOD-05.07.01<br/>(Workload Identity & SPIFFE/SPIRE)"]
    end

    M_IDF --> M_HASH
    M_HASH --> M_MFA
    M_IDF --> M_KERB
    M_MFA --> M_OAUTH
    M_OAUTH --> M_SSO
    M_KERB --> M_ATT
    M_OAUTH --> M_ATT
    M_SSO --> M_WORK
```

---

## 6. Implementation Checklist & Status
- [x] Domain-05 Master Specification Created & Frozen.
- [x] 7 Core Engineering Branches Defined.
- [x] 8 Universal Technical Modules mapped and scheduled.
- [x] Complete Mermaid DAG populated.
