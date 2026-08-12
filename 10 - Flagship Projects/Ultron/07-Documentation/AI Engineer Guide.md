# AI Engineer Guide (AI Engineer Onboarding)

> Document ID: DOC-ULTRON-L0-AI-ENGINEER-ONBOARDING
> Document Name: AI Engineer Guide (AI Engineer Onboarding)
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.AIEngineer
> Architecture Version: 1.0.0
> Abstraction Level: L0 Vision & AI Domain Process
> Parent Document: DOC-ULTRON-L0-TEAM-RESPONSIBILITIES
> Child Documents: DOC-ULTRON-L3-AI-PIPELINE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0, AVR v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document serves as the authoritative **AI Engineer Onboarding Guide** for software engineers responsible for the `Intelligence` and `Knowledge` modules within Project Ultron.

It provides exhaustive guidance across 26 architectural, operational, and domain topics—defining the reasoning engine, planning engine, memory, embeddings, local model management, and boundary rules to allow a new AI Engineer to begin contributing immediately without violating system architecture contracts.

---

# Scope

### Included in Scope
- Complete 26-section operational guide for the AI Engineer leading the `Intelligence` and `Knowledge` subsystems.
- AI pipeline mechanics (Prompt Engineering, Context Management, Reasoning Loop, RAG Vector Search, Embedding Models, Model Runtime Management).
- Clear authority matrix: Independent modifications vs. Architecture Lead signoffs vs. Read-Only prohibited modules.
- Testing, prompt benchmarks, learning paths, and first implementation task sequences.

### Excluded from Scope
- Host OS process dispatching or Linux binary sandbox execution (owned by `System` and `Security` modules).
- General git configuration (covered in `Developer Guide.md`).

---

# Engineering Question

**How does the AI Engineer manage local LLM runtimes, prompt assembly, RAG retrieval, embeddings, and neural reasoning within the Intelligence module boundaries without violating system architecture contracts?**

---

# Context

This document occupies abstraction level **L0 Vision & AI Domain Process** in `07-Documentation/`.

It derives from `00-Overview/06 Team Responsibilities.md` and provides operational guidance for `01-Architecture/06 AI Pipeline.md`.

```
00-Overview/06 Team Responsibilities.md (Team Ownership Matrix)
↓
07-Documentation/AI Engineer Guide.md (AI Engineer Onboarding - This Document)
↓
01-Architecture/06 AI Pipeline.md (L3 AI Pipeline Specification)
```

---

# 1. Purpose of Ultron
Project Ultron is a self-hosted, offline-first AI engineering platform built on Linux. It combines artificial intelligence reasoning with systems engineering, giving developers an intelligent assistant capable of understanding, reasoning, planning, and proposing system actions safely.

---

# 2. Overall Architecture & 3. Layered Architecture

Ultron is structured into a strict 5-layer system stack. The AI engine operates strictly within **Layer 3**:

```
+-----------------------------------------------------------------------+
| Layer 1: PRESENTATION LAYER (CLI, Web Presentation Gateway)           |
+-----------------------------------------------------------------------+
                                   | (RequestContext)
                                   v
+-----------------------------------------------------------------------+
| Layer 2: GATEWAY & ROUTING LAYER (PayloadNormalizer, EventBus Router) |
+-----------------------------------------------------------------------+
                                   | (RequestContext)
                                   v
+-----------------------------------------------------------------------+
| Layer 3: REASONING & PLANNING LAYER (AI Engineer Domain)              |
| - Intelligence: AIRuntime, PromptAssembler, OutputParser, Planner     |
| - Knowledge: VectorStore, Local Embeddings, ContextRetriever, Memory   |
+-----------------------------------------------------------------------+
                                   | (ExecutionPlan Proposal)
                                   v
+-----------------------------------------------------------------------+
| Layer 4: SECURITY & EXECUTION LAYER (Architecture Lead Domain)        |
| - SecurityEngine (Policy Evaluator) | ProcessSupervisor (Linux Exec)  |
+-----------------------------------------------------------------------+
                                   | (Sanitized Tool Call)
                                   v
+-----------------------------------------------------------------------+
| Layer 5: INFRASTRUCTURE & HOST OS LAYER (Linux OS Kernel & Tools)     |
+-----------------------------------------------------------------------+
```

---

# 4. Module Relationships

- `Interface` passes `RequestContext` down to `Intelligence`.
- `Intelligence` queries `Knowledge` for local vector embeddings and sliding window memory.
- `Intelligence` (`Planner`) formulates an `ExecutionPlan` proposal and submits it to `Security`.
- `Security` evaluates the proposal and routes approved tool steps to `System` for process execution.
- `System` returns `ToolCallResult` output streams back up to `Intelligence` and `Interface`.

---

# 5. Responsibilities of the AI Engineer
- Lead developer and domain owner for `Intelligence` and `Knowledge` modules.
- Responsible for local LLM inference integration (Ollama, llama.cpp), local vector embeddings, prompt engineering, RAG retrieval algorithms, and structured JSON output parsing.

---

# 6. Responsibilities of the Architecture Lead
- Domain owner for `Core`, `System`, `Security`, `Automation`, `Interface`, `Development`, and `Extensions`.
- Responsible for system architecture, `SecurityEngine` policy evaluation, process sandbox execution, and contract boundary definitions.

---

# 7. Shared Responsibilities
- Defining tool schema hints inside `ToolRegistry` (`Development` module).
- Maintaining Pydantic data contract models (`RequestContext`, `ExecutionPlan`, `ToolCallResult`).
- Verifying end-to-end integration test execution.

---

# 8. Intelligence Module Overview

The `Intelligence` module (`backend/intelligence/`) houses all artificial intelligence reasoning components:
- `AIRuntime`: Local LLM connection pool manager.
- `PromptAssembler`: System prompt constructor.
- `OutputParser`: JSON repair and Pydantic validator.
- `Planner`: Workstream execution plan builder.

---

# 9. Reasoning Engine & 10. Planning Engine

- **Reasoning Engine**: Uses local LLMs to evaluate user prompts, session context, and retrieved RAG snippets, producing natural language reasoning chains.
- **Planning Engine**: Converts raw reasoning chains into structured Directed Acyclic Graphs (DAGs) represented as Pydantic `ExecutionPlan` objects matching `10 API Contracts.md`.

---

# 11. Memory & 12. Context Management

- **Memory Engine (`backend/knowledge/memory/`)**: Maintains short-term sliding context windows and session turn history (`RequestContext` history).
- **Context Management (`backend/knowledge/context/`)**: Dynamically trims and prioritizes prompt tokens to fit within the local LLM context window (e.g., 4096 / 8192 tokens).

---

# 13. Prompt Management

- System prompts live in version-controlled markdown/template files in `backend/intelligence/prompts/`.
- Prompts enforce strict anti-hallucination guardrails and instruct the model to produce **ONLY valid JSON** matching the `ExecutionPlan` schema.

---

# 14. Embeddings & 15. Model Manager

- **Embeddings (`backend/knowledge/embeddings/`)**: Uses lightweight, local sentence-transformers models (e.g., `all-MiniLM-L6-v2`) running on CPU/NPU worker pools to vectorize code files and notes.
- **Model Manager (`backend/intelligence/runtime/`)**: Manages HTTP clients for local inference daemons (Ollama / llama.cpp on `127.0.0.1:11434`), handling fallback retries and model unloading.

---

# 16. Tool Selection

- The model selects tools exclusively from the active tool schema array injected into the prompt by `ToolRegistry`.
- The AI Engineer MUST ensure system prompts clearly describe tool parameters so the LLM chooses the correct tool for the user prompt.

---

# 17. Runtime Boundaries & 18. Security Boundaries

- **Runtime Boundary**: The AI pipeline operates strictly in user space via local HTTP/socket calls to Ollama.
- **Security Boundary**: The AI pipeline has **ZERO direct execution power**. The AI engine CANNOT spawn Linux processes or run shell scripts directly; it can only propose an `ExecutionPlan` to the `SecurityEngine`.

---

# 19. Communication with Other Modules & 20. Interfaces to Respect

- **Inbound Interface**: Must receive `RequestContext` from `Interface` gateway.
- **Outbound Interface**: Must output Pydantic `ExecutionPlan` proposals matching `10 API Contracts.md`.
- **Knowledge Interface**: Must query `IVectorStore` for semantic search snippets.

---

# 21. What the AI Engineer Can Modify (Independent Authority)

- ✅ System prompt templates and few-shot examples inside `backend/intelligence/prompts/`.
- ✅ Local embedding models, chunking logic, and similarity thresholds in `backend/knowledge/`.
- ✅ Hyperparameters (`temperature`, `top_p`, `max_tokens`) in `backend/intelligence/runtime/`.
- ✅ Output parsing regex and JSON repair functions in `backend/intelligence/parser/`.

---

# 22. What Requires Architecture Approval

- ⚠️ Modifying `RequestContext`, `ExecutionPlan`, or `ToolCallResult` JSON schema contracts.
- ⚠️ Introducing third-party cloud LLM API calls (violates offline-first invariant).
- ⚠️ Modifying inter-subsystem data payload definitions (`10 API Contracts.md`).

---

# 23. Testing Expectations & 24. Documentation Expectations

- **Testing**: Maintain `pytest tests/intelligence/` mock test suites and benchmark prompt templates against 50+ evaluation prompts to ensure `< 2%` parsing failure rate.
- **Documentation**: Any changes to AI pipeline flows MUST be updated in `01-Architecture/06 AI Pipeline.md` following `ADS v1.0` schema.

---

# 25. Recommended Learning Path & 26. First Implementation Tasks

### Learning Path
1. Read `00 Vision.md` and `06 Team Responsibilities.md`.
2. Read `01-Architecture/06 AI Pipeline.md` and `10 API Contracts.md`.
3. Inspect `backend/intelligence/` and `backend/knowledge/` packages.

### First Implementation Tasks (Milestone 2)
1. Implement `AIRuntime` Ollama client in `backend/intelligence/runtime/client.py`.
2. Build sentence-transformers embedding loader in `backend/knowledge/embeddings/local.py`.
3. Build `OutputParser` JSON repair utility in `backend/intelligence/parser/json_repair.py`.

---

# Responsibilities

### Primary Responsibilities
- Provide a comprehensive 26-section onboarding reference for AI Engineers.
- Establish strict authority boundaries between AI reasoning and system execution.
- Maintain alignment between local AI model capabilities and system architecture contracts.

### Secondary Responsibilities
- Guide prompt engineering benchmark reviews.
- Mentoring new AI contributors on local RAG optimization.

### Out of Scope
- Writing host Linux process execution code.
- Managing database server deployments.

---

# Relationships

### Child Of
- `DOC-ULTRON-L0-TEAM-RESPONSIBILITIES` (`00-Overview/06 Team Responsibilities.md`).

### References
- `10 - Flagship Projects/Ultron/01-Architecture/06 AI Pipeline.md` (AI Pipeline Spec)
- `10 - Flagship Projects/Ultron/01-Architecture/10 API Contracts.md` (Interface Contracts)

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `06 Team Responsibilities.md` | Parent Document | Incoming | Defines team ownership and authority boundaries | Mandatory |
| `06 AI Pipeline.md` | Child Spec | Outgoing | Detailed L3 execution spec for AI pipeline | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Offline-First Invariant**: All LLM inference and embedding calculations MUST operate locally without cloud network calls.
- **Zero Execution Power**: The AI module CANNOT execute tools directly; tool execution power resides exclusively in the `System` module via `SecurityEngine`.

---

# References

- `10 - Flagship Projects/Ultron/00-Overview/06 Team Responsibilities.md` (Team Ownership)
- `10 - Flagship Projects/Ultron/01-Architecture/06 AI Pipeline.md` (AI Pipeline Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Integration of local GGUF quantization tuning guides for llama.cpp.
- Specification of speculative decoding benchmarks for local NPU hardware.
