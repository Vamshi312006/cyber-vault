# 06 AI Pipeline

> Document ID: DOC-ULTRON-L3-AI-PIPELINE
> Document Name: 06 AI Pipeline
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.AIEngineer
> Architecture Version: 1.0.0
> Abstraction Level: L3 AI Architecture
> Parent Document: DOC-ULTRON-L3-TOOL-EXECUTION-FLOW
> Child Documents: DOC-ULTRON-L2-SECURITY-ARCHITECTURE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the AI reasoning pipeline and prompt assembly architecture within Project Ultron.

It defines how natural language user requests are enriched with local RAG knowledge context, formatted into system-aligned prompts, submitted to local LLM inference runtimes, and parsed into validated execution plans (`ExecutionPlan`).

---

# Scope

### Included in Scope
- AI reasoning pipeline stages (Context Assembly $\rightarrow$ Prompt Construction $\rightarrow$ LLM Inference $\rightarrow$ Output Parsing).
- Prompt template structure and system guideline injection.
- LLM response parsing and JSON tool proposal extraction.

### Excluded from Scope
- Low-level neural network weight training or model fine-tuning algorithms.
- Cloud LLM API wrapper implementations (local inference runtimes are primary).

---

# Engineering Question

**How does the AI pipeline ingest context, format prompts, execute local LLM inference, and parse structured reasoning into execution plans?**

---

# Context

This document occupies abstraction level **L3 AI Architecture** in the Ultron architecture hierarchy.

It derives from `05 Tool Execution Flow.md` and provides the AI processing specification for `07 Security Architecture.md` and `08 Runtime Architecture.md`.

```
05 Tool Execution Flow.md (L3 Tool Execution Flow)
↓
06 AI Pipeline.md (L3 AI Pipeline - This Document)
↓
07 Security Architecture.md (L2 Security Architecture)
```

---

# Architecture

The AI Pipeline in Ultron processes tasks through a 4-stage sequential reasoning loop:

```
[RequestContext + Session Memory]
               |
               v
+-----------------------------------------------------------------------+
|  Stage 1: Context Aggregation & RAG Injection                         |
|  - Retrieve working session history from MemoryEngine                 |
|  - Query VectorStore for codebase & documentation embeddings          |
|  - Aggregate available tool schemas from ToolRegistry                 |
+-----------------------------------------------------------------------+
               |
               v
+-----------------------------------------------------------------------+
|  Stage 2: Structured Prompt Construction                              |
|  - Inject System Persona & Anti-Hallucination Guidelines              |
|  - Format active RAG context blocks & tool schema definitions         |
|  - Append user prompt into final LLM input payload                    |
+-----------------------------------------------------------------------+
               |
               v
+-----------------------------------------------------------------------+
|  Stage 3: Local LLM Model Inference                                   |
|  - Submit prompt to AIRuntime (Ollama / llama.cpp HTTP endpoint)      |
|  - Apply temperature, top_p, and max_tokens generation parameters      |
|  - Stream or capture raw completion response                          |
+-----------------------------------------------------------------------+
               |
               v
+-----------------------------------------------------------------------+
|  Stage 4: Response Parsing & ExecutionPlan Formulation                |
|  - Parse raw text into structured JSON payload                        |
|  - Extract reasoning steps and proposed tool call arrays             |
|  - Validate output schema to construct typed ExecutionPlan           |
+-----------------------------------------------------------------------+
```

---

# Responsibilities

### Primary Responsibilities
- Specify the structured 4-stage AI reasoning pipeline.
- Guarantee that prompt construction includes system guidelines, available tool schemas, and local RAG context.
- Ensure raw LLM completions are parsed into strictly typed `ExecutionPlan` data structures.

### Secondary Responsibilities
- Provide prompt assembly guidelines for AI engineers.
- Guide error handling for LLM syntax errors or invalid JSON completions.

### Out of Scope
- Writing model quantization C++ code.
- Managing GPU driver installations on host Linux systems.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L2-SECURITY-ARCHITECTURE` (`07 Security Architecture.md`).

### Child Of
- `DOC-ULTRON-L3-TOOL-EXECUTION-FLOW` (`05 Tool Execution Flow.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `05 Tool Execution Flow.md` | Parent Document | Incoming | Provides execution context driving AI pipeline planning | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes layer hierarchy constraints | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Offline Inference Priority**: AI reasoning must prioritize local LLM inference engines (Ollama, llama.cpp).
- **Strict Parsing Gate**: Raw LLM output MUST be successfully parsed into a validated `ExecutionPlan` structure before reaching the Security Engine.
- **Explainability**: The prompt construction stage must preserve step-by-step reasoning blocks to ensure decision transparency.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/05 Tool Execution Flow.md` (Tool Execution Flow)
- `10 - Flagship Projects/Ultron/01-Architecture/03 Request Lifecycle.md` (Request Lifecycle Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Specification of multi-model routing (e.g., routing small tasks to fast local models and complex tasks to larger models).
- Integration of speculative decoding hooks for local LLM acceleration.
