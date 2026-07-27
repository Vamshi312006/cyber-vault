# 01 Overall Architecture

> Document ID: DOC-ULTRON-L1-OVERALL-ARCHITECTURE
> Document Name: 01 Overall Architecture
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L1 Layered Architecture
> Parent Document: DOC-ULTRON-L1-SYSTEM-OVERVIEW
> Child Documents: DOC-ULTRON-L2-COMPONENT-ARCHITECTURE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the layered architecture topology and cross-cutting structural boundaries governing Project Ultron.

It defines the canonical 5-layer system stack (`Presentation`, `Gateway & Controller`, `Reasoning & Planning`, `Security & Execution`, `Infrastructure & OS`), ensuring strict directional dependency flows and layer isolation.

---

# Scope

### Included in Scope
- Canonical 5-layer system architecture topology.
- Directional layer dependency rules and communication constraints.
- Cross-cutting concerns (logging, configuration, security tracing).

### Excluded from Scope
- Individual component interface method signatures (owned by L2/L6 specifications).
- Source code implementation details (owned by L7 implementation docs).

---

# Engineering Question

**What layered architecture topology and cross-cutting boundary rules govern the Ultron system software stack?**

---

# Context

This document occupies abstraction level **L1 Layered Architecture** in the Ultron architecture hierarchy.

It derives from `00 System Overview.md` and provides the structural framework for `02 Component Architecture.md` and `08 Runtime Architecture.md`.

```
00 System Overview.md (L1 System Overview)
↓
01 Overall Architecture.md (L1 Layered Architecture - This Document)
↓
02 Component Architecture.md (L2 Component Architecture)
```

---

# Architecture

The Ultron system stack is structured into five strictly layered tiers, adhering to the Architecture Reference Model (`ARM v1.0`):

```
+-----------------------------------------------------------------------+
|  Layer 1: PRESENTATION LAYER (CLI, Web UI, Extensions Hook)           |
+-----------------------------------------------------------------------+
                                   | (RequestContext Payload)
                                   v
+-----------------------------------------------------------------------+
|  Layer 2: GATEWAY & CONTROLLER LAYER (Gateway, Router, Normalizer)     |
+-----------------------------------------------------------------------+
                                   | (Normalized Context)
                                   v
+-----------------------------------------------------------------------+
|  Layer 3: REASONING & PLANNING LAYER (AI Engine, Planner, RAG Memory) |
+-----------------------------------------------------------------------+
                                   | (ExecutionPlan Proposal)
                                   v
+-----------------------------------------------------------------------+
|  Layer 4: SECURITY & EXECUTION LAYER (Security Engine, Dispatcher)    |
+-----------------------------------------------------------------------+
                                   | (Sanitized Tool Call)
                                   v
+-----------------------------------------------------------------------+
|  Layer 5: INFRASTRUCTURE & OS LAYER (System Handlers, Linux OS, KV)   |
+-----------------------------------------------------------------------+
```

### Layer Interaction Invariants
1. **Strict Downward Invocation**: Upper layers may call lower layers; lower layers must never directly invoke upper layers (callback events use decoupled event bus notifications).
2. **Contract Isolation**: Inter-layer communication uses immutable data contracts (`RequestContext`, `ExecutionPlan`, `ToolCallResult`).
3. **Layer Bypassing Prohibited**: Presentation layer components (Layer 1) cannot bypass Gateway (Layer 2) or Security (Layer 4) to invoke OS execution directly (Layer 5).

---

# Responsibilities

### Primary Responsibilities
- Define the 5-tier layered architecture for Project Ultron.
- Enforce strict directional dependency rules across layers.
- Maintain layer isolation to allow independent component testing and replacement.

### Secondary Responsibilities
- Guide static code analysis rules to catch layer boundary violations.
- Inform refactoring efforts to decouple monolithic legacy functions into layered components.

### Out of Scope
- Defining specific function parameters or C/Python data structure definitions.
- Managing operating system process scheduling.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L2-COMPONENT-ARCHITECTURE` (`02 Component Architecture.md`).

### Child Of
- `DOC-ULTRON-L1-SYSTEM-OVERVIEW` (`00 System Overview.md`).

### References
- `ARM v1.0` (Architecture Reference Model Layering).
- `AKM v1.0` (Architecture Knowledge Model Ontology).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `00 System Overview.md` | Parent Document | Incoming | Provides macro-system overview driving layer definitions | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes canonical 5-layer architecture rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Zero Upward Dependencies**: No package in Layer N may import or depend on packages in Layer N-1 or higher.
- **Security Gate Enforcement**: Layer 4 (Security & Execution) MUST evaluate all proposals from Layer 3 before reaching Layer 5.
- **Layer Independence**: Swapping Layer 1 interfaces (e.g., CLI for Web) must require 0 modifications to Layer 3 or Layer 4.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/00 System Overview.md` (System Overview)
- `00 - Meta/AI Collaboration/ARM.md` (Architecture Reference Model)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Formalization of remote gRPC layer transport for distributed multi-node layer execution.
- Integration of compile-time dependency graph enforcement rules.
