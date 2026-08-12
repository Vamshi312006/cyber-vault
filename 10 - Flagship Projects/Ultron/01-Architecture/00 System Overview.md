# 00 System Overview

> Document ID: DOC-ULTRON-L1-SYSTEM-OVERVIEW
> Document Name: 00 System Overview
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L1 System Architecture
> Parent Document: DOC-ULTRON-L0-VISION
> Child Documents: DOC-ULTRON-L1-OVERALL-ARCHITECTURE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document provides the authoritative L1 System Architecture overview for Project Ultron.

It defines the macro-level subsystem decomposition, high-level interaction patterns, and boundary relationships between the nine core platform modules (`Core`, `System`, `Interface`, `Intelligence`, `Automation`, `Security`, `Knowledge`, `Development`, `Extensions`).

---

# Scope

### Included in Scope
- High-level system topology and subsystem decomposition (L1 abstraction).
- Core interaction contracts between Interface, Planner, Security, and Host Execution layers.
- System-wide architectural invariants.

### Excluded from Scope
- Detailed internal module code or class definitions (owned by L4 module specs).
- Specific API JSON schema payloads (owned by `10 API Contracts.md`).
- Hardware or operating system kernel source code (owned by host Linux OS).

---

# Engineering Question

**How is the Ultron system high-level architecture structured into primary subsystems, and how do they interact at the L1 abstraction level?**

---

# Context

This document occupies abstraction level **L1 System Architecture** in the Ultron architecture hierarchy.

It bridges the high-level L0 Vision (`00-Overview/`) with detailed L2 Component Architectures (`02 Component Architecture.md`) and L3 Execution Lifecycles (`03 Request Lifecycle.md`).

```
L0 Vision (00-Overview/00 Vision.md)
↓
L1 System Overview (This Document)
↓
L2 Component Architecture (02 Component Architecture.md)
```

---

# Architecture

Ultron is architected as a modular, event-driven system operating on top of host Linux operating systems. The system comprises nine primary modules arranged across four conceptual layers:

```
+-----------------------------------------------------------------------+
|                        INTERFACE SUBSYSTEM                            |
|             Gateway  |  CLI  |  Web Presentation Layer               |
+-----------------------------------------------------------------------+
                                   |
                                   v
+-----------------------------------------------------------------------+
|                     INTELLIGENCE & AUTOMATION                         |
|      Intelligence Engine (AI Runtime)  |  Automation (Planner)        |
+-----------------------------------------------------------------------+
                                   |
                                   v
+-----------------------------------------------------------------------+
|                     SECURITY & KNOWLEDGE LAYER                        |
|       Security Engine (Policy Gate)   |  Knowledge (RAG / Memory)    |
+-----------------------------------------------------------------------+
                                   |
                                   v
+-----------------------------------------------------------------------+
|                    CORE, SYSTEM & EXTENSIONS LAYER                    |
|   Core (Event Bus) | System (Host Linux Exec) | Extensions (Plugins)  |
+-----------------------------------------------------------------------+
```

### High-Level Subsystem Roles
1. **Interface Subsystem**: Normalizes external requests into standard `RequestContext` contracts.
2. **Intelligence Subsystem**: Drives LLM reasoning, prompt construction, and context integration.
3. **Automation Subsystem**: Constructs structured `ExecutionPlan` pipelines containing proposed tool calls.
4. **Security Subsystem**: Performs mandatory `SecurityEngine` policy gating, parameter sanitization, and privilege verification.
5. **Knowledge Subsystem**: Manages session state, working memory, and local vector RAG document indexes.
6. **Core Subsystem**: Provides system initialization, configuration loading, and event bus message routing.
7. **System Subsystem**: Dispatches authorized tool executions safely to host Linux OS binaries.
8. **Development Subsystem**: Excludes developer tools, schemas, and Tool Registry contracts.
9. **Extensions Subsystem**: Loads third-party plugins via standardized capability interface hooks.

---

# Responsibilities

### Primary Responsibilities
- Define the L1 macro-architecture of Project Ultron.
- Enforce strict layer boundary rules (higher layers invoke lower layers via contracts).
- Preserve architectural alignment across all subsystem interfaces.

### Secondary Responsibilities
- Serve as the primary reference for multi-module integration discussions.
- Guide boundary checks during system refactoring.

### Out of Scope
- Writing module internal source code or function definitions.
- Defining database table columns or file storage paths.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L1-OVERALL-ARCHITECTURE` (`01 Overall Architecture.md`).
- `DOC-ULTRON-L2-COMPONENT-ARCHITECTURE` (`02 Component Architecture.md`).

### Child Of
- `DOC-ULTRON-L0-VISION` (`00 Vision.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ARM v1.0` (Architecture Reference Model).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `00 Vision.md` | Parent Document | Incoming | Inherits overarching system vision and principles | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes layer hierarchy constraints | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Single Gateway Entry**: All external requests must enter through the Interface Subsystem.
- **Mandatory Security Gate**: No ExecutionPlan step may bypass Security Subsystem policy evaluation.
- **Decoupled Messaging**: Inter-subsystem communication must use explicit interface contracts or the Core event bus.

---

# References

- `10 - Flagship Projects/Ultron/00-Overview/00 Vision.md` (Overarching Vision)
- `10 - Flagship Projects/Ultron/01-Architecture/03 Request Lifecycle.md` (Request Lifecycle Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Definition of multi-node L1 cluster topology for enterprise deployments.
- Specification of distributed event bus bridging across remote Linux daemons.
