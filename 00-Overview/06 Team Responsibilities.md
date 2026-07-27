# 06 Team Responsibilities

> Document ID: DOC-ULTRON-L0-TEAM-RESPONSIBILITIES
> Document Name: 06 Team Responsibilities
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.Vision
> Architecture Version: 1.0.0
> Abstraction Level: L0 Vision
> Parent Document: DOC-ULTRON-L0-SCOPE
> Child Documents: DOC-ULTRON-L1-SYSTEM-OVERVIEW
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document establishes the official ownership and team responsibility matrix for Project Ultron.

It explicitly maps every approved module and technical domain to its authoritative engineering lead, eliminating ambiguity in architectural governance, code ownership, and review authority.

---

# Scope

### Included in Scope
- Module ownership mappings for all nine core platform modules and enterprise extensions.
- Ownership division within shared technical modules (Intelligence, Knowledge, Development).
- Governance rules for architecture reviews, pull request approvals, and design authority.

### Excluded from Scope
- Individual developer task assignments or sprint board management.
- HR organizational hierarchies or administrative reporting lines.
- External vendor management protocols.

---

# Engineering Question

**Who owns each architectural module and technical domain within Project Ultron?**

---

# Context

This document occupies abstraction level **L0 Vision** in the Ultron architecture hierarchy.

It derives from `03 Scope.md` and provides the governance framework for module implementations across all lower-abstraction layers.

```
03 Scope.md (L0 System Scope)
↓
06 Team Responsibilities.md (L0 Ownership Matrix - This Document)
↓
L1 System Architecture & L4 Module Specifications
```

---

# Architecture

Module ownership within Project Ultron is divided between the **Architecture Lead** and the **AI Engineer**:

### 1. Exclusive Subsystem Ownership (Architecture Lead)
The **Architecture Lead** holds complete, exclusive architectural and implementation authority over the following core subsystems:
- **Core Subsystem**: System initialization, event loop, and global configuration.
- **System Subsystem**: Host Linux OS interactions, process supervision, system calls, and sandboxing.
- **Interface Subsystem**: Gateway, payload normalization, CLI, and presentation interfaces.
- **Automation Subsystem**: Workflow planning, ExecutionPlan construction, and tool dispatching.
- **Security Subsystem**: Security Engine, permission verification, payload sanitization, and policy enforcement.
- **Extensions Subsystem**: Plugin loading, capability registration, and interface extension contracts.
- **Enterprise Subsystem**: Enterprise cluster governance and multi-tenant access control.

### 2. Shared Subsystem Ownership Matrix
For shared technical domains (**Intelligence**, **Knowledge**, and **Development**), responsibility is explicitly partitioned based on domain expertise:

| Shared Module | Architecture Lead Responsibilities | AI Engineer Responsibilities |
| :--- | :--- | :--- |
| **Intelligence** | Architecture, Runtime Integration, Security Gating, Execution Supervision, System Design | LLM Integration, Neural Network Interfaces, Model Optimization, AI Runtime |
| **Knowledge** | Storage Engine Architecture, Memory Security, Session Lifecycle, Data Integrity | Embeddings Generation, Vector Store Retrieval (RAG), Semantic Indexing |
| **Development** | Tool Registry Contracts, System Utility Architecture, Testing Supervision | AI Tool Definition, Prompt Schemas, Evaluation Harnesses |

---

# Responsibilities

### Primary Responsibilities
- Establish clear, non-overlapping ownership for every system module and component.
- Govern architectural change approvals and review pipelines.
- Maintain strict single-source-of-truth ownership across the repository.

### Secondary Responsibilities
- Guide pull request routing and code review assignments.
- Resolve cross-module integration disputes between engineering roles.

### Out of Scope
- Assigning daily engineering tasks or managing sprint backlogs.
- Performing non-technical administrative reviews.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L1-SYSTEM-OVERVIEW` (`00 System Overview.md`).

### Child Of
- `DOC-ULTRON-L0-SCOPE` (`03 Scope.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `A2A-ADP v1.0` (AI-to-AI Architecture Delegation Protocol).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `03 Scope.md` | Parent Document | Incoming | Defines module boundaries that require ownership assignment | Mandatory |
| `AKM v1.0` | Ontology | Incoming | Provides single-ownership ontology rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and ownership contracts | Mandatory |

---

# Constraints

- **Single Ownership Principle**: Every architectural component must have exactly one authoritative owner.
- **Ownership Invariance**: Ownership assignments may not be modified without explicit Architecture AI approval.
- **Review Boundary**: Code changes to exclusively owned modules must be approved by the designated Architecture Lead.

---

# References

- `10 - Flagship Projects/Ultron/00-Overview/03 Scope.md` (System Scope)
- `00 - Meta/AI Collaboration/AKM.md` (Architecture Knowledge Model)
- `00 - Meta/AI Collaboration/A2A-ADP.md` (Delegation Protocol)

---

# Future Scope

- Expansion of team ownership matrices for distributed open-source working groups.
- Integration of automated CODEOWNERS file generation linked to this specification.
