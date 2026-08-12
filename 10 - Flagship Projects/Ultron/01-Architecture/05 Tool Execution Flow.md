# 05 Tool Execution Flow

> Document ID: DOC-ULTRON-L3-TOOL-EXECUTION-FLOW
> Document Name: 05 Tool Execution Flow
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L3 Execution Architecture
> Parent Document: DOC-ULTRON-L3-DATA-FLOW
> Child Documents: DOC-ULTRON-L3-AI-PIPELINE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the tool execution flow and security dispatch pipeline within Project Ultron.

It defines the exact sequence of checks, parameter validations, privilege escalations, sandbox isolations, and process executions performed whenever an AI-generated or user-requested tool call is dispatched to the host Linux OS.

---

# Scope

### Included in Scope
- Step-by-step tool execution sequence (Resolution $\rightarrow$ Validation $\rightarrow$ Escalation $\rightarrow$ Dispatch $\rightarrow$ Capture).
- Security Engine policy evaluation and sanitization hooks.
- Process isolation, timeout enforcement, and error capture mechanisms.

### Excluded from Scope
- Source code implementation of specific tools (e.g., Python `subprocess.run` wrappers).
- UI rendering of tool execution progress bars.

---

# Engineering Question

**What are the precise execution sequence, security validation stages, and process isolation steps involved when dispatching a tool call to the host OS?**

---

# Context

This document occupies abstraction level **L3 Execution Architecture** in the Ultron architecture hierarchy.

It derives from `04 Data Flow.md` and provides the execution flow specification for `06 AI Pipeline.md` and `07 Security Architecture.md`.

```
04 Data Flow.md (L3 Systemic Data Flow)
↓
05 Tool Execution Flow.md (L3 Tool Execution Flow - This Document)
↓
07 Security Architecture.md (L2/L3 Security Architecture)
```

---

# Architecture

Tool execution in Ultron follows a strict, non-bypassable 5-stage dispatch flow:

```
[Tool Call Proposal from Planner]
       |
       v
+-----------------------------------------------------------------------+
|  Stage 1: Tool Registry Schema Resolution                            |
|  - Verify tool_name exists in ToolRegistry                             |
|  - Validate parameters against JSON Schema contract                   |
+-----------------------------------------------------------------------+
       | (Schema Valid)
       v
+-----------------------------------------------------------------------+
|  Stage 2: Security Engine Policy Evaluation                           |
|  - Check privilege requirements (Read, Write, Execute, Elevated)     |
|  - Sanitize parameters (path traversal, flag injection, shell escapes)|
|  - Evaluate security policies (Block/Allow)                          |
+-----------------------------------------------------------------------+
       | (Policy Passed)
       v
+-----------------------------------------------------------------------+
|  Stage 3: Interactive Privilege Escalation Check                      |
|  - If operation is Elevated/Destructive -> Prompt User Confirmation   |
|  - If User Denies -> Return SECURITY_REJECTED to Planner             |
+-----------------------------------------------------------------------+
       | (User Approved / Normal Privilege)
       v
+-----------------------------------------------------------------------+
|  Stage 4: Isolated Host OS Process Execution                         |
|  - Spawn subprocess via ProcessSupervisor in restricted context       |
|  - Enforce timeout bounds (e.g., 30s max execution time)              |
|  - Stream stdout / stderr to execution buffer                        |
+-----------------------------------------------------------------------+
       |
       v
+-----------------------------------------------------------------------+
|  Stage 5: Tool Result Capture & Audit Logging                        |
|  - Wrap exit code, stdout, stderr into immutable ToolCallResult       |
|  - Record execution metrics and audit trace log                       |
+-----------------------------------------------------------------------+
```

---

# Responsibilities

### Primary Responsibilities
- Specify the non-bypassable tool execution sequence.
- Guarantee that every tool call undergoes schema resolution, security policy check, and parameter sanitization.
- Ensure host process execution remains bounded by resource limits and timeouts.

### Secondary Responsibilities
- Provide execution trace references for security auditing.
- Guide error handling for tool failures, timeouts, and permission rejections.

### Out of Scope
- Writing specific Linux shell commands or script files.
- Managing OS user permissions outside the application boundary.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L3-AI-PIPELINE` (`06 AI Pipeline.md`).

### Child Of
- `DOC-ULTRON-L3-DATA-FLOW` (`04 Data Flow.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `04 Data Flow.md` | Parent Document | Incoming | Provides data flow pipeline driving tool execution | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes layer execution constraints | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Zero Unchecked Subprocesses**: Direct invocation of host OS commands without passing through the 5-stage dispatch flow is strictly prohibited.
- **Timeout Bound**: Every tool process execution MUST have a hard timeout configuration to prevent system hangs.
- **Immutable Result**: The generated `ToolCallResult` must be immutable once captured.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/04 Data Flow.md` (Data Flow Spec)
- `10 - Flagship Projects/Ultron/01-Architecture/03 Request Lifecycle.md` (Request Lifecycle Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Integration of containerized sandbox execution environments (e.g., Docker / Firecracker microVMs).
- Specification of resource quota metrics per tool process (CPU % / RAM limits).
