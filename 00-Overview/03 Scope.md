# 03 Scope

> Document ID: DOC-ULTRON-L0-SCOPE
> Document Name: 03 Scope
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.Vision
> Architecture Version: 1.0.0
> Abstraction Level: L0 Vision
> Parent Document: DOC-ULTRON-L0-REQUIREMENTS
> Child Documents: DOC-ULTRON-L0-FEATURES
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document defines the strict operational and architectural scope of Project Ultron.

It explicitly demarcation lines between core platform capabilities, supported system interfaces, and out-of-scope domain boundaries, preventing scope creep and maintaining focus on core platform engineering goals.

---

# Scope

### Included in Scope
- Core platform architecture and subsystem decomposition.
- In-scope functional modules: Core, System, Interface, Intelligence, Automation, Security, Knowledge, Development, and Extensions.
- Native Linux OS integration and local LLM execution boundaries.

### Excluded from Scope
- Cloud-hosted SaaS management platforms (out of scope for baseline Ultron).
- Windows/macOS native kernel integrations (Linux OS is the primary target runtime).
- Proprietary closed-source model training or fine-tuning pipelines.

---

# Engineering Question

**What are the precise architectural boundaries, inclusions, and exclusions for the Ultron platform?**

---

# Context

This document occupies abstraction level **L0 Vision** in the Ultron architecture hierarchy.

It builds upon `02 Requirements.md` and provides the scope boundary that informs `04 Features.md` and `06 Team Responsibilities.md`.

```
02 Requirements.md (L0 System Requirements)
↓
03 Scope.md (L0 System Scope - This Document)
↓
04 Features.md (L0 System Features)
```

---

# Architecture

The architectural scope of Ultron is structured into three clear boundary tiers:

### 1. In-Scope Subsystems (Core Platform Boundary)
The platform scope strictly encompasses nine primary modules:
- **Core**: System initialization, event bus, and global configuration.
- **System**: Host Linux OS interaction, process supervision, and system calls.
- **Interface**: Gateway, CLI, Web presentation, and payload normalization.
- **Intelligence**: AI Runtime, local LLM orchestration, and prompt formatting.
- **Automation**: Workflow planning, execution plan building, and task dispatching.
- **Security**: Security Engine, permission verification, and policy enforcement.
- **Knowledge**: Memory Engine, session state, local vector RAG, and document indexing.
- **Development**: Tool registry, developer utilities, and testing hooks.
- **Extensions**: Plugin loader, interface extensions, and third-party capability registration.

### 2. Out-of-Scope Capabilities (Explicit Boundaries)
The following domain areas are explicitly out of scope for the core platform architecture:
- **Direct GUI Desktop Rendering**: Ultron exposes API/CLI/Web interfaces; direct X11/Wayland window compositing is excluded.
- **Proprietary Hardware Drivers**: Operating system kernel driver development is out of scope; Ultron relies on standard Linux kernel interfaces.
- **External Cloud Dependencies**: Operations requiring mandatory external cloud endpoints are excluded from the core architecture.

### 3. Future Scope Boundaries (Enterprise Expansion)
- **Enterprise Subsystem**: Multi-node cluster orchestration and enterprise role-based access control (RBAC) are deferred to future enterprise specifications.

---

# Responsibilities

### Primary Responsibilities
- Establish clear operational and architectural boundaries for Project Ultron.
- Prevent unapproved feature additions or boundary drift.
- Define explicit inclusion and exclusion lists for engineering teams.

### Secondary Responsibilities
- Guide technical review committees during milestone evaluations.
- Clarify boundary questions for open-source contributors.

### Out of Scope
- Defining specific module internal functions or data models.
- Managing project schedules, developer assignments, or sprint boards.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L0-FEATURES` (`04 Features.md`).

### Child Of
- `DOC-ULTRON-L0-REQUIREMENTS` (`02 Requirements.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ARM v1.0` (Architecture Reference Model).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `02 Requirements.md` | Parent Document | Incoming | Provides system requirements that define boundary scope | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes layer hierarchy constraints | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compiler rules | Mandatory |

---

# Constraints

- **Boundary Invariance**: Scope modifications require formal Architecture AI approval.
- **Linux Focus**: All system integration boundaries must target Linux operating environments.
- **Single Source of Truth**: Modules not listed in the approved module list may not be introduced without updating this specification.

---

# References

- `10 - Flagship Projects/Ultron/00-Overview/02 Requirements.md` (System Requirements)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)
- `00 - Meta/AI Collaboration/AKM.md` (Architecture Knowledge Model)

---

# Future Scope

- Extension of scope boundaries for multi-node Linux cluster deployments.
- Definition of plugin sandbox scope constraints for third-party extensions.
