# Developer Guide (Developer Onboarding)

> Document ID: DOC-ULTRON-L0-DEVELOPER-ONBOARDING
> Document Name: Developer Guide (Developer Onboarding)
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L0 Vision & Process
> Parent Document: DOC-ULTRON-L0-DASHBOARD
> Child Documents: None (Onboarding Guide)
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0, AVR v1.0, Documentation Rules v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document serves as the official **Developer Onboarding Guide** for engineers joining Project Ultron.

It provides a comprehensive overview of the project vision, repository structure, development philosophy, engineering standards, Git/PR workflows, common pitfalls, and a structured first-week roadmap to enable new engineers to contribute effectively without breaking architectural contracts.

---

# Scope

### Included in Scope
- Comprehensive 16-section onboarding roadmap for new software engineers and system architects.
- Development philosophy (Architecture-First, Security-First, Incremental Delivery).
- Repository layout, document reading order, engineering standards, and Git/PR processes.
- Communication workflows, common engineering mistakes, and first-week objectives.

### Excluded from Scope
- Code-level implementation syntax or specific database driver setup.
- Hardware procurement or server rack installation.

---

# Engineering Question

**How do software engineers onboard, navigate the repository, collaborate with AI agents, and contribute code under Ultron's architecture-first methodology?**

---

# Context

This document occupies abstraction level **L0 Vision & Process** in `07-Documentation/`.

It derives from `00-Overview/09 Dashboard.md` and serves as the primary onboarding entry point for all software developers and systems architects joining Project Ultron.

```
00-Overview/09 Dashboard.md (Master Vault Dashboard)
↓
07-Documentation/Developer Guide.md (Developer Onboarding - This Document)
↓
01-Architecture/ Specifications & backend/ Implementation
```

---

# 1. Project Introduction

### What Ultron Is
Project Ultron is an open-source, offline-first AI engineering platform for Linux. It integrates artificial intelligence, Linux systems engineering, modular software architecture, and cybersecurity into a unified, self-hosted system layer.

### What Ultron Is Not
- Ultron is **NOT** a simple wrapper around cloud LLM APIs (OpenAI/Anthropic).
- Ultron is **NOT** an un-monitored autonomous agent that executes unchecked terminal commands.
- Ultron is **NOT** a monolithic script repository.

### Vision
To provide a secure, explainable, offline-first platform capable of understanding, reasoning, planning, and executing complex system engineering tasks alongside human engineers.

### Mission
To establish an immutable, architecture-first AI engineering platform where artificial intelligence enhances developer velocity while remaining bounded by strict security policies and human authorization.

### Long-Term Goals
- **Platform Integrity**: Build reusable, modular platform abstractions over ad-hoc scripts.
- **Offline Sovereignty**: Run 100% locally on host Linux systems with zero cloud network dependencies.
- **Security-First Execution**: Guarantee zero unverified or un-sanitized host OS executions.

---

# 2. Repository Overview

The repository is organized into distinct, non-overlapping directory trees:

- `00 - Meta/AI Collaboration/`: Meta-specifications governing AI-to-AI delegation, document schemas, ontologies, and compilation rules (`A2A-ADP`, `AKM`, `ADS`, `AVR`, `ARM`, `ACP`, `AVM`, `AQL`, `Documentation Rules`).
- `00-Overview/`: L0 Vision documentation vault (`00 Vision` through `10 Glossary`).
- `01-Architecture/`: L1-L6 Architectural specifications, system overview, component maps, data flows, API contracts, and sequence diagrams.
- `07-Documentation/`: High-level operational guides (`Developer Guide`, `AI Engineer Guide`, `Installation`, `User Guide`).
- `backend/`: Single root Python application source tree containing subsystem implementations (`foundation/`, `interface/`, `intelligence/`, `planner/`, `security/`, `knowledge/`, `system/`, `development/`, `extensions/`).
- `tests/`: Automated unit, integration, security, and prompt benchmark test harnesses.

---

# 3. Development Philosophy

1. **Architecture Before Implementation**: Code is derivative of frozen architectural specifications. You must NEVER write source code for a module or interface that does not have an approved specification in `01-Architecture/`.
2. **Documentation Before Coding**: Documentation is not an afterthought; it is the single-source-of-truth blueprint.
3. **Security-First Design**: Security is not a feature added later. Every tool execution proposal MUST pass through the `SecurityEngine` policy gate.
4. **Modular Development**: High cohesion within modules; low coupling across boundaries. Inter-subsystem communication must use explicit contracts (`10 API Contracts.md`).
5. **Incremental Implementation**: Deliver capability through sequential milestone gates (M0 through M5) without skipping prerequisites.

---

# 4. Recommended Reading Order

New developers MUST read repository documentation in the following exact sequence before writing code:

```
Step 1: 00-Overview/00 Vision.md           (Core Vision & 8 Principles)
   ↓
Step 2: 00-Overview/06 Team Responsibilities.md (Ownership Matrix)
   ↓
Step 3: 00 - Meta/AI Collaboration/Complete Protocol.md (AI Collaboration Rules)
   ↓
Step 4: 01-Architecture/00 System Overview.md   (L1 Subsystem Decomposition)
   ↓
Step 5: 01-Architecture/01 Overall Architecture.md (5-Layer Stack & Rules)
   ↓
Step 6: 01-Architecture/02 Component Architecture.md (Component Dependency Map)
   ↓
Step 7: 01-Architecture/03 Request Lifecycle.md  (Context & Execution Plan Flow)
   ↓
Step 8: 01-Architecture/07 Security Architecture.md (Security Engine Gating)
   ↓
Step 9: 01-Architecture/10 API Contracts.md      (Shared Interface Contracts)
```

---

# 5. Repository Navigation

- **Locating Architecture Specs**: Navigate to `01-Architecture/`. Look for document numbers matching the abstraction level (`00 System Overview` for L1, `02 Component Architecture` for L2, `10 API Contracts` for L6).
- **Locating Module Code**: Navigate to `backend/<module-name>/`. Every package strictly maps to its logical module (e.g., `backend/security/` for SecurityEngine).
- **Locating Test Fixtures**: Navigate to `tests/<module-name>/`. Unit and integration tests mirror the `backend/` directory structure.

---

# 6. Engineering Standards

- **Single Ownership**: Concepts are owned by exactly one module and one document owner as specified in `06 Team Responsibilities.md`.
- **Traceability**: Every system request, log line, and tool call MUST carry a valid `request_id` and `trace_id`.
- **Decoupled Architecture**: Direct circular imports across packages are strictly prohibited. Use the `EventBus` for decoupled event notifications.

---

# 7. Coding Standards

- **Python Version**: Primary codebase runs on **Python 3.11+**.
- **Type Hints**: All function signatures MUST include explicit Python type annotations (`def execute_step(plan: ExecutionPlan) -> ToolCallResult:`).
- **Data Modeling**: All inter-subsystem data payloads MUST use Pydantic `BaseModel` schemas.
- **Async Concurrency**: Non-blocking asynchronous IO (`asyncio`) MUST be used for network, event bus, and disk operations.

---

# 8. Documentation Standards

- **ADS Compliance**: All markdown documentation MUST adhere strictly to the Architecture Document Schema (`ADS v1.0`).
- **Single Engineering Question**: Every document MUST clearly state and answer exactly one engineering question.
- **Metadata Headers**: Every document MUST include standard header metadata (Document ID, Version, Status, Owner, Parent/Child links).

---

# 9. Git Workflow & 10. Branching Strategy

- **Main Branch (`master` / `main`)**: Contains production-ready, frozen specifications and code. Direct commits are prohibited.
- **Branch Naming Convention**:
  - `arch/<spec-name>` — Specification or documentation updates (e.g., `arch/security-spec-update`).
  - `feat/<module-name>` — Subsystem feature implementation (e.g., `feat/planner-dag-builder`).
  - `fix/<issue-id>` — Bug or security vulnerability fix (e.g., `fix/path-sanitizer-leak`).
- **Commit Message Format**:
  - `[ARCH] <description>` — For architectural changes.
  - `[FEAT] <description>` — For feature implementations.
  - `[FIX]  <description>` — For bug fixes.

---

# 11. Code Review & 12. Pull Request Process

### PR Checklist Requirements
1. **Module Owner Approval**: PR must be reviewed and approved by the domain owner specified in `06 Team Responsibilities.md`.
2. **Zero Architectural Drift**: PR must not introduce unapproved module boundaries or bypass `SecurityEngine` checks.
3. **Automated Test Pass**: All unit, security, and integration tests in `tests/` must pass cleanly.
4. **Documentation Sync**: If code interfaces change, corresponding specs in `01-Architecture/` MUST be updated in the same PR.

---

# 13. Testing Expectations

- **Unit Test Coverage**: Maintain `> 90%` code coverage for core libraries (`ConfigManager`, `EventBus`, `OutputParser`).
- **Security Validation Tests**: 100% test pass required for security policy evaluation (0 path traversal or flag injection leaks allowed).
- **Prompt Benchmark Suite**: AI prompts must undergo regression benchmarking against test prompt suites to ensure `< 2%` parsing failure rates.

---

# 14. Communication Workflow

- **Architectural Clarification**: If a specification is ambiguous or incomplete, do NOT guess. File an "Architecture Clarification Request" to the Architecture Lead.
- **AI Tool Pair Programming**: Treat AI assistants (Antigravity, Copilot) as **Implementation Engines**. AI tools MUST NOT make architectural decisions or modify frozen contracts independently.

---

# 15. Common Mistakes to Avoid

1. ❌ **Writing Code Before Spec**: Creating Python code before the architectural document in `01-Architecture/` is approved and frozen.
2. ❌ **Bypassing SecurityEngine**: Invoking host OS processes directly without routing tool calls through the `SecurityEngine` policy gate.
3. ❌ **Cross-Module Coupling**: Importing internal private classes from another subsystem instead of using shared interface contracts (`10 API Contracts.md`).
4. ❌ **Modifying Unowned Modules**: Modifying files outside your assigned domain without Architecture Lead signoff (`06 Team Responsibilities.md`).

---

# 16. First Week Roadmap for New Engineers

- **Day 1: Orientation & Reading**:
  - Read `00 Vision.md`, `06 Team Responsibilities.md`, and `Complete Protocol.md`.
  - Clone repository and verify local environment prerequisites (Python 3.11+, pytest).
- **Day 2: Architecture Deep Dive**:
  - Read `00 System Overview.md`, `01 Overall Architecture.md`, `02 Component Architecture.md`, and `07 Security Architecture.md`.
  - Trace a sample request path through `03 Request Lifecycle.md`.
- **Day 3: Development Setup & Test Suite Execution**:
  - Run existing test suites: `pytest tests/`.
  - Inspect backend directory layout (`backend/`).
- **Day 4: First Good First Task**:
  - Pick a minor issue or spec clarification task.
  - Create a branch (`feat/<task-name>`), write unit tests, and implement changes.
- **Day 5: PR Submission & Review**:
  - Submit Pull Request following PR checklist.
  - Participate in code review with domain owner and merge contribution.

---

# Responsibilities

### Primary Responsibilities
- Provide an authoritative 16-section developer onboarding reference for Project Ultron.
- Ensure new team members understand the Architecture-First methodology.
- Maintain alignment between onboarding standards and repository practices.

### Secondary Responsibilities
- Guide pull request reviews and onboarding mentorship.
- Eliminate common engineering mistakes during initial contributions.

### Out of Scope
- Writing individual module code.
- Managing user account passwords.

---

# Relationships

### Child Of
- `DOC-ULTRON-L0-DASHBOARD` (`00-Overview/09 Dashboard.md`).

### References
- `10 - Flagship Projects/Ultron/00-Overview/00 Vision.md` (Overarching Vision)
- `10 - Flagship Projects/Ultron/00-Overview/06 Team Responsibilities.md` (Team Ownership)
- `00 - Meta/AI Collaboration/Complete Protocol.md` (AI Protocols)

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `00-Overview/09 Dashboard.md` | Parent Document | Incoming | Master index driving onboarding discovery | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compiler contracts | Mandatory |
| `Documentation Rules v1.0` | Rules | Operational | Governs documentation creation boundaries | Mandatory |

---

# Constraints

- **Onboarding Mandatory**: New developers MUST complete the 16-section onboarding guide before submitting code PRs.
- **Zero Drift**: Onboarding practices MUST enforce 100% compliance with frozen architecture specifications.

---

# References

- `10 - Flagship Projects/Ultron/00-Overview/00 Vision.md` (Overarching Vision)
- `10 - Flagship Projects/Ultron/01-Architecture/00 System Overview.md` (System Overview)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Integration of interactive CLI onboarding scripts (`ultron onboard`).
- Development of automated onboarding progress tracking checklists for engineering leads.
