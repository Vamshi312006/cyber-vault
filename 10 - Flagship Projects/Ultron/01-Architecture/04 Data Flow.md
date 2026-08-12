# 04 Data Flow

> Document ID: DOC-ULTRON-L3-DATA-FLOW
> Document Name: 04 Data Flow
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L3 Execution Architecture
> Parent Document: DOC-ULTRON-L3-REQUEST-LIFECYCLE
> Child Documents: DOC-ULTRON-L3-TOOL-EXECUTION-FLOW
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document defines the systemic data flow and payload transformation pipelines across Project Ultron.

It specifies how raw user inputs evolve into structured request contexts, AI reasoning prompts, proposed execution plans, security-sanitized tool calls, and formatted output responses.

---

# Scope

### Included in Scope
- Data payload transformations along the request lifecycle path.
- State mutation points and data immutability contracts.
- Payload structures across subsystem boundaries.

### Excluded from Scope
- Low-level database column types or binary C struct layouts (owned by L6 specs).
- Detailed tool-specific parameter schemas (owned by `10 API Contracts.md`).

---

# Engineering Question

**How does data flow through the Ultron system from initial user input to final response display, and what transformations occur at each stage?**

---

# Context

This document occupies abstraction level **L3 Execution Architecture** in the Ultron architecture hierarchy.

It derives from `03 Request Lifecycle.md` and provides data flow specification for `05 Tool Execution Flow.md` and `06 AI Pipeline.md`.

```
03 Request Lifecycle.md (L3 Request Lifecycle)
↓
04 Data Flow.md (L3 Systemic Data Flow - This Document)
↓
05 Tool Execution Flow.md (L3 Tool Execution Flow)
```

---

# Architecture

Data transformation in Ultron proceeds through a deterministic 6-stage pipeline:

```
[Raw User Input]
       |
       v  (Stage 1: Ingestion & Context Normalization)
[RequestContext] {request_id, trace_id, user_prompt, session_id}
       |
       v  (Stage 2: Knowledge Augmentation)
[Augmented Context] {RequestContext + RAG Embeddings + Working Memory}
       |
       v  (Stage 3: Reasoning & Planning)
[ExecutionPlan] {plan_id, reasoning_steps, proposed_tool_calls[]}
       |
       v  (Stage 4: Security Policy Gate)
[Sanitized ExecutionPlan] {ExecutionPlan + SecurityAuditLogs + Approvals}
       |
       v  (Stage 5: Tool Execution)
[ToolCallResult] {step_id, exit_code, stdout, stderr, execution_time}
       |
       v  (Stage 6: Output Formatting)
[ResponsePayload] {rendered_markdown, audit_summary, status}
```

### Data Pipeline Stage Definitions
1. **Stage 1 (Ingestion)**: Gateway receives raw CLI args or HTTP payload, generates unique `request_id` and `trace_id`, and wraps data in immutable `RequestContext`.
2. **Stage 2 (Augmentation)**: Knowledge Subsystem queries VectorStore and session memory to attach relevant codebase RAG context.
3. **Stage 3 (Reasoning)**: AIRuntime processes prompt; Planner formats raw LLM output into typed `ExecutionPlan`.
4. **Stage 4 (Security Gate)**: SecurityEngine evaluates each step in `ExecutionPlan`, attaches policy evaluation metadata, and strips un-sanitized flags.
5. **Stage 5 (Execution)**: Dispatcher routes sanitized tool calls to System Subsystem, capturing execution stdout/stderr in `ToolCallResult`.
6. **Stage 6 (Formatting)**: ResponseFormatter aggregates tool outputs into final user-facing `ResponsePayload`.

---

# Responsibilities

### Primary Responsibilities
- Specify systemic data flow pipelines and payload transformations.
- Enforce immutability for core tracing fields (`request_id`, `trace_id`).
- Ensure data payload structures adhere to typed contract schemas.

### Secondary Responsibilities
- Provide data flow reference for debugging state mutations.
- Inform data privacy and logging redaction policies.

### Out of Scope
- Writing JSON serializer source code.
- Implementing database storage engines.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L3-TOOL-EXECUTION-FLOW` (`05 Tool Execution Flow.md`).

### Child Of
- `DOC-ULTRON-L3-REQUEST-LIFECYCLE` (`03 Request Lifecycle.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `03 Request Lifecycle.md` | Parent Document | Incoming | Provides request lifecycle phases driving data flow | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes data flow boundary rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Trace ID Preservation**: The `trace_id` generated in Stage 1 must be propagated untouched across all 6 data flow stages.
- **Explicit Serialization**: Data passed across subsystem boundaries must use explicit JSON/Pydantic serialization schemas.
- **Sanitization Invariant**: Raw unsanitized data from Stage 3 MUST NOT enter Stage 5 directly without Stage 4 security validation.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/03 Request Lifecycle.md` (Request Lifecycle Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)
- `00 - Meta/AI Collaboration/AKM.md` (Architecture Knowledge Model)

---

# Future Scope

- Definition of binary stream data flow pipelines for large file processing.
- Specification of encrypted payload serialization for remote enterprise cluster transport.
