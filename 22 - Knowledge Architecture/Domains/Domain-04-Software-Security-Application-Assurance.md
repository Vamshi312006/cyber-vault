---
id: "DOM-04"
title: "Domain 04: Software Security - Master Knowledge Architecture"
type: "domain-specification"
domain_id: "04"
maintainer: "Cyber Act Software Security Team"
last_audited: "2026-07-29"
---

# Domain 04: Software Security — Master Knowledge Architecture

## 1. Domain Identity & Overview
- **Domain ID**: `DOM-04`
- **Canonical Name**: Software Security
- **Ontology Parent**: `KU-CYBER`
- **Domain Status**: `Active`

`DOM-04` establishes secure software engineering practices, vulnerability primitive classifications, memory safety models, secure API architectures, static/dynamic application analysis, coverage-guided fuzzing, and software supply chain security standards across Cyber Act.

---

## 2. Scope & Exclusion Boundaries
- **In Scope**: Secure design principles, input validation, memory safety (C/C++ vs Rust/Go), vulnerability classes (OWASP/CWE), secure API architectures (REST/GraphQL/gRPC), SAST/DAST/IAST, coverage-guided fuzzing (AFL++), SBOM, and secure SDLC pipelines.
- **Explicit Exclusions**:
  - Identity, Authentication & Passwords (governed by `DOM-05`).
  - Authorization, RBAC & IAM (governed by `DOM-06`).
  - Endpoint OS Security (governed by `DOM-07`).
  - Pipeline Execution Infrastructure Security (governed by `DOM-11`).

---

## 3. Branch Decomposition Matrix

Domain 04 is partitioned into **6 Core Engineering Branches**:

```
Domain-04: Software Security
├── Branch 04.1: Secure Software Design (sec-design)
├── Branch 04.2: Secure Coding Practices (sec-coding)
├── Branch 04.3: Software Vulnerabilities (sec-vulnerabilities)
├── Branch 04.4: Secure Application Architecture (sec-app-arch)
├── Branch 04.5: Secure SDLC & Supply Chain Security (sec-sdlc-supply)
└── Branch 04.6: Software Security Testing (sec-testing)
```

---

## 4. Branch & Module Breakdown

### Branch 04.1: Secure Software Design (`sec-design`)
- **`MOD-04.01.01`**: Security Design Principles & Threat Modeling (STRIDE, PASTA, Trust Boundaries)
- **`MOD-04.01.02`**: Defense-in-Depth, Least Privilege & Attack Surface Reduction

### Branch 04.2: Secure Coding Practices (`sec-coding`)
- **`MOD-04.02.01`**: Input Validation, Output Encoding & Contextual Sanitization
- **`MOD-04.02.02`**: Secure File Handling, Error Management & Secret Sanitization in Code

### Branch 04.3: Software Vulnerabilities (`sec-vulnerabilities`)
- **`MOD-04.03.01`**: Memory Corruption Primitives (Buffer Overflow, Use-After-Free, Integer Overflow)
- **`MOD-04.03.02`**: Command & Query Injection Flaws (SQLi, Command Injection, LDAP)
- **`MOD-04.03.03`**: Web Application Vulnerabilities (XSS, CSRF, SSRF, IDOR, Insecure Deserialization)

### Branch 04.4: Secure Application Architecture (`sec-app-arch`)
- **`MOD-04.04.01`**: Secure API Design (REST, GraphQL, gRPC & Payload Sanitization)
- **`MOD-04.04.02`**: Microservice & Service Communication Security (mTLS, Message Queues)

### Branch 04.5: Secure SDLC & Supply Chain Security (`sec-sdlc-supply`)
- **`MOD-04.05.01`**: Secure SDLC Integration, Code Review & SAST/SCA Analysis
- **`MOD-04.05.02`**: Software Supply Chain Security, SBOM (CycloneDX/SPDX) & Dependency Isolation

### Branch 04.6: Software Security Testing (`sec-testing`)
- **`MOD-04.06.01`**: Automated Security Testing, DAST & Coverage-Guided Fuzzing (AFL++/libFuzzer)

---

## 5. Module Dependency Graph (Mermaid DAG)

```mermaid
graph TD
    subgraph Layer 0: Design & Coding Fundamentals
        M_DES["MOD-04.01.01<br/>(Security Design & Threat Modeling)"]
        M_COD["MOD-04.02.01<br/>(Input Validation & Output Encoding)"]
    end

    subgraph Layer 1: Vulnerabilities & Memory Safety
        M_MEM["MOD-04.03.01<br/>(Memory Corruption & Buffer Overflows)"]
        M_INJ["MOD-04.03.02<br/>(SQLi & Command Injection)"]
        M_WEB["MOD-04.03.03<br/>(XSS, CSRF & SSRF)"]
    end

    subgraph Layer 2: Architecture & APIs
        M_API["MOD-04.04.01<br/>(Secure API REST/GraphQL/gRPC)"]
        M_MIC["MOD-04.04.02<br/>(Microservice Communication & Queues)"]
    end

    subgraph Layer 3: SDLC, Testing & Fuzzing
        M_SDLC["MOD-04.05.01<br/>(Secure SDLC & SAST)"]
        M_SBOM["MOD-04.05.02<br/>(Software Supply Chain & SBOM)"]
        M_FUZZ["MOD-04.06.01<br/>(Coverage-Guided Fuzzing AFL++)"]
    end

    M_DES --> M_COD
    M_COD --> M_INJ
    M_COD --> M_WEB
    M_MEM --> M_API
    M_INJ --> M_API
    M_API --> M_MIC
    M_COD --> M_SDLC
    M_SDLC --> M_SBOM
    M_MEM --> M_FUZZ
```

---

## 6. Implementation Checklist & Status
- [x] Domain-04 Master Specification Updated & Frozen.
- [x] 6 Core Engineering Branches Defined.
- [x] 11 Universal Technical Modules mapped and scheduled.
- [x] Complete Mermaid DAG populated.
