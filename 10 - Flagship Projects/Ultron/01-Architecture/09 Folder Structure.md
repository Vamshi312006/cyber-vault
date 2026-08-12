# 09 Folder Structure

> Document ID: DOC-ULTRON-L4-FOLDER-STRUCTURE
> Document Name: 09 Folder Structure
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L4 Folder Structure
> Parent Document: DOC-ULTRON-L2-RUNTIME-ARCHITECTURE
> Child Documents: DOC-ULTRON-L6-API-CONTRACTS
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the authoritative physical directory layout and package structure for Project Ultron.

It defines how the nine logical modules and cross-cutting layers map directly to repository paths, establishing clear guidelines for file placement, module boundaries, and source tree organization.

---

# Scope

### Included in Scope
- Source repository tree organization (`backend/`, `foundation/`, `planner/`, etc.).
- Subsystem module folder mappings and file placement rules.
- Documentation vault directory hierarchy (`00-Overview/` through `12-Open Source/`).

### Excluded from Scope
- Temporary build directory contents or compiled byte-code artifacts (`.pyc`, `__pycache__`).
- Individual source code file line contents (owned by L7 implementation docs).

---

# Engineering Question

**What physical directory layout and package structure map the logical modules of Project Ultron to filesystem paths?**

---

# Context

This document occupies abstraction level **L4 Folder Structure** in the Ultron architecture hierarchy.

It derives from `08 Runtime Architecture.md` and provides filesystem layout mappings for `10 API Contracts.md` and `11 Data Models.md`.

```
08 Runtime Architecture.md (L2 Runtime Architecture)
↓
09 Folder Structure.md (L4 Folder Structure - This Document)
↓
10 API Contracts.md (L6 API Contracts)
```

---

# Architecture

The physical repository structure of Ultron maps strictly to its approved modular architecture:

```
ultron/
├── 00-Overview/                  # L0 Overview Documentation Vault
│   ├── 00 Vision.md
│   ├── 01 Goals.md
│   ├── 02 Requirements.md
│   ├── 03 Scope.md
│   ├── 04 Features.md
│   ├── 05 Success Metrics.md
│   ├── 06 Team Responsibilities.md
│   ├── 07 Milestones.md
│   ├── 08 Roadmap.md
│   ├── 09 Dashboard.md
│   └── 10 Glossary.md
├── 01-Architecture/              # L1-L6 System Architecture Vault
│   ├── 00 System Overview.md
│   ├── 01 Overall Architecture.md
│   ├── 01 Vision.md
│   ├── 02 Component Architecture.md
│   ├── 03 Request Lifecycle.md
│   ├── 04 Data Flow.md
│   ├── 05 Tool Execution Flow.md
│   ├── 06 AI Pipeline.md
│   ├── 07 Security Architecture.md
│   ├── 08 Runtime Architecture.md
│   ├── 09 Folder Structure.md
│   ├── 10 API Contracts.md
│   ├── 11 Data Models.md
│   └── 12 Deployment Architecture.md
├── backend/                      # Single Root Application Backend
│   ├── main.py                   # Single Entry Point Daemon
│   ├── foundation/               # Core Subsystem & Shared Models
│   │   ├── config/               # ConfigManager & Env Settings
│   │   ├── events/               # EventBus & System Notification
│   │   └── models/               # Shared Pydantic Contracts
│   ├── interface/                # Interface Subsystem (Gateway/CLI/Web)
│   ├── intelligence/             # Intelligence Subsystem (AIRuntime)
│   ├── planner/                  # Automation Subsystem (Planner & Execution)
│   ├── security/                 # Security Subsystem (SecurityEngine)
│   ├── knowledge/                # Knowledge Subsystem (Memory & VectorStore)
│   ├── system/                   # System Subsystem (Linux OS Execution)
│   ├── development/              # Development Subsystem (ToolRegistry)
│   └── extensions/               # Extensions Subsystem (Plugin Loader)
└── tests/                        # Automated Verification Harnesses
```

---

# Responsibilities

### Primary Responsibilities
- Declare the canonical directory layout for Project Ultron.
- Enforce strict file placement rules to prevent folder pollution.
- Guarantee that every module has a dedicated, non-overlapping directory path.

### Secondary Responsibilities
- Provide filesystem references for build scripts and packaging tools.
- Guide developers on where to add new files or components.

### Out of Scope
- Defining Python class method implementations.
- Configuring Linux file permission bits (`chmod`/`chown`).

---

# Relationships

### Parent Of
- `DOC-ULTRON-L6-API-CONTRACTS` (`10 API Contracts.md`).

### Child Of
- `DOC-ULTRON-L2-RUNTIME-ARCHITECTURE` (`08 Runtime Architecture.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `08 Runtime Architecture.md` | Parent Document | Incoming | Provides process model driving package organization | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes layer directory structure rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Single Root Backend**: Source code MUST reside within the single `backend/` root directory (no parallel microservices).
- **Subsystem Boundary Isolation**: Files in `backend/security/` may not be placed inside `backend/interface/`.
- **No Orphan Folders**: Directories outside the approved structure may not be created without Architecture AI approval.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/08 Runtime Architecture.md` (Runtime Architecture)
- `10 - Flagship Projects/Ultron/01-Architecture/00 System Overview.md` (System Overview)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Definition of enterprise multi-repo subtree integration layouts.
- Specification of plugin distribution folder structures.
