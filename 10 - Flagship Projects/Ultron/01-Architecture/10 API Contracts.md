# 10 API Contracts (Interface Contracts Specification)

> Document ID: DOC-ULTRON-L6-INTERFACE-CONTRACTS
> Document Name: 10 API Contracts (Interface Contracts Specification)
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L6 API & Interface Contracts
> Parent Document: DOC-ULTRON-L4-FOLDER-STRUCTURE
> Child Documents: DOC-ULTRON-L6-DATA-MODELS
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0, AVR v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the formal **Interface Contracts** for Project Ultron.

It defines every shared interface boundary across 10 core categories before source code implementation begins, providing explicit Request/Response structures, Error contracts, Lifecycles, Versioning, Constraints, Security Considerations, and Future Compatibility guarantees without implementing code.

---

# Scope

### Included in Scope
- Comprehensive 10-part interface contract specification (`Request`, `Response`, `Tool`, `Runtime`, `Event`, `Memory`, `Plugin`, `Security`, `Configuration`, `Logging`).
- Operational metadata for every interface across all 14 mandatory fields: Purpose, Owner, Provider, Consumer, Inputs, Outputs, Request Structure, Response Structure, Errors, Lifecycle, Versioning, Constraints, Security Considerations, and Future Compatibility.

### Excluded from Scope
- Concrete Python class bodies or C library code (owned by L7 implementation docs).
- Operating system kernel socket driver bindings.

---

# Engineering Question

**What are the formal interface contracts, data payload schemas, and operational constraints governing inter-module communication across Project Ultron?**

---

# Context

This document occupies abstraction level **L6 API & Interface Contracts** in the Ultron architecture hierarchy.

It derives from `09 Folder Structure.md` and provides mandatory interface contract boundaries for `11 Data Models.md` and `12 Deployment Architecture.md`.

```
09 Folder Structure.md (L4 Folder Structure)
↓
10 API Contracts.md (L6 Interface Contracts Specification - This Document)
↓
11 Data Models.md (L6 Data Models)
```

---

# Subsystem Interface Specifications

---

## 1. Request Interface (`IRequestContext`)

- **Purpose**: Normalizes raw user prompts, CLI flags, and Web client payloads into an immutable context object.
- **Owner**: `Ultron.ArchitectureLead` (`Interface` Subsystem)
- **Provider**: `Gateway` / `PayloadNormalizer`
- **Consumer**: `Planner`, `AIRuntime`, `SecurityEngine`, `MemoryEngine`
- **Inputs**: `raw_prompt` (string), `user_id` (string), `session_id` (string)
- **Outputs**: Normalized `RequestContext` object
- **Request Structure**:
  ```json
  {
    "request_id": "req-12345-uuid-v4",
    "trace_id": "trace-67890-uuid-v4",
    "session_id": "sess-abcde-uuid-v4",
    "user_prompt": "Analyze repository security and summarize main.py",
    "timestamp": "2026-07-27T22:00:00Z",
    "metadata": { "client_type": "cli", "auth_level": "user" }
  }
  ```
- **Response Structure**: Returns immutable `RequestContext` object instance.
- **Errors**: `INVALID_PROMPT_EMPTY`, `MALFORMED_METADATA_JSON`, `UNAUTHORIZED_SESSION`.
- **Lifecycle**: Created at Gateway entry point; immutable; destroyed when response is sent.
- **Versioning**: `v1.0.0` (Semantic Versioning).
- **Constraints**: `request_id` and `trace_id` MUST be valid UUID v4 strings.
- **Security Considerations**: Strips raw control characters and enforces UTF-8 string encoding.
- **Future Compatibility**: Metadata dictionary accepts arbitrary key-values for multi-agent trace propagation.

---

## 2. Response Interface (`IResponsePayload`)

- **Purpose**: Formats execution results, reasoning summaries, and status codes for presentation layers.
- **Owner**: `Ultron.ArchitectureLead` (`Interface` Subsystem)
- **Provider**: `ResponseFormatter` / `Planner`
- **Consumer**: `CLI` / `WebPresentationGateway` / User
- **Inputs**: `request_id` (string), `status` (enum), `tool_results` (array), `summary` (string)
- **Outputs**: Formatted `ResponsePayload` markdown object
- **Request Structure**: Standard payload rendering trigger.
- **Response Structure**:
  ```json
  {
    "request_id": "req-12345-uuid-v4",
    "status": "SUCCESS",
    "rendered_markdown": "### Security Summary\nFile main.py analyzed...",
    "execution_time_ms": 142.5,
    "audit_trail_ref": "audit-998877"
  }
  ```
- **Errors**: `FORMATTING_RENDER_ERROR`, `PAYLOAD_TOO_LARGE`.
- **Lifecycle**: Instantiated upon plan completion or execution failure; transmitted to client.
- **Versioning**: `v1.0.0`.
- **Constraints**: Output format MUST compile cleanly into terminal ANSI or GitHub-Flavored Markdown.
- **Security Considerations**: Redacts sensitive environment variables or credentials before rendering.
- **Future Compatibility**: Supports streaming chunk payloads via WebSocket handlers.

---

## 3. Tool Interface (`ITool`)

- **Purpose**: Standardized execution interface for host system capabilities registered in `ToolRegistry`.
- **Owner**: `Ultron.ArchitectureLead` (`Development` Subsystem)
- **Provider**: Registered Tool Extensions
- **Consumer**: `Dispatcher` (`Automation`) / `ProcessSupervisor` (`System`)
- **Inputs**: `sanitized_parameters` (dict), `execution_context` (RequestContext)
- **Outputs**: `ToolCallResult` object
- **Request Structure**:
  ```json
  {
    "step_id": "step-1",
    "tool_name": "file_read",
    "sanitized_parameters": { "file_path": "/workspace/main.py" }
  }
  ```
- **Response Structure**:
  ```json
  {
    "step_id": "step-1",
    "exit_code": 0,
    "stdout": "import os\nimport sys...",
    "stderr": "",
    "execution_time_ms": 14.2,
    "status": "SUCCESS"
  }
  ```
- **Errors**: `TOOL_EXECUTION_TIMEOUT`, `NON_ZERO_EXIT_CODE`, `PARAMETER_TYPE_MISMATCH`.
- **Lifecycle**: Discovered at startup; executed statelessly inside sandboxed subprocesses.
- **Versioning**: `v1.0.0`.
- **Constraints**: Execution MUST enforce a strict timeout (default: 30s).
- **Security Considerations**: Execution MUST be pre-cleared by `SecurityEngine`.
- **Future Compatibility**: Allows containerized execution runtimes (Docker/Firecracker).

---

## 4. Runtime Interface (`IAIRuntime`)

- **Purpose**: Abstracts local LLM inference engines (Ollama, llama.cpp) behind a uniform generation interface.
- **Owner**: `Ultron.AIEngineer` (`Intelligence` Subsystem)
- **Provider**: `AIRuntime` Daemon Client
- **Consumer**: `Planner` (`Automation`)
- **Inputs**: `prompt` (string), `hyperparameters` (dict)
- **Outputs**: `LLMCompletionResult` object
- **Request Structure**:
  ```json
  {
    "model_name": "llama3:8b-instruct",
    "prompt": "<system>...</system><user>...</user>",
    "temperature": 0.2,
    "top_p": 0.9,
    "max_tokens": 2048
  }
  ```
- **Response Structure**:
  ```json
  {
    "raw_text": "{\n  \"plan_id\": \"plan-1\",\n  \"proposed_tool_calls\": []\n}",
    "prompt_tokens": 420,
    "completion_tokens": 85,
    "inference_time_ms": 612.0
  }
  ```
- **Errors**: `INFERENCE_DAEMON_OFFLINE`, `CONTEXT_WINDOW_EXCEEDED`, `LLM_TIMEOUT`.
- **Lifecycle**: Persistent HTTP connection pool active throughout daemon session.
- **Versioning**: `v1.0.0`.
- **Constraints**: MUST operate locally on `127.0.0.1` without cloud API network calls.
- **Security Considerations**: Prevents prompt injection leaking host OS environment secrets.
- **Future Compatibility**: Supports streaming token callbacks for real-time UI rendering.

---

## 5. Event Interface (`IEventBus`)

- **Purpose**: Asynchronous publish/subscribe event routing across core modules.
- **Owner**: `Ultron.ArchitectureLead` (`Core` Subsystem)
- **Provider**: `EventBus` Engine
- **Consumer**: Subsystem Event Handlers / Audit Loggers
- **Inputs**: `event_name` (string), `payload` (dict), `sender` (string)
- **Outputs**: Asynchronous dispatch acknowledgment
- **Request Structure**:
  ```json
  {
    "event_name": "TOOL_EXECUTION_COMPLETED",
    "sender": "Dispatcher",
    "trace_id": "trace-67890-uuid-v4",
    "timestamp": "2026-07-27T22:00:01Z",
    "payload": { "step_id": "step-1", "exit_code": 0 }
  }
  ```
- **Response Structure**: Async event dispatch return (void).
- **Errors**: `HANDLER_EXCEPTION`, `EVENT_QUEUE_FULL`.
- **Lifecycle**: System daemon startup to shutdown.
- **Versioning**: `v1.0.0`.
- **Constraints**: Event handlers MUST NOT block the main `asyncio` event loop.
- **Security Considerations**: Event payloads MUST NOT contain un-sanitized credential tokens.
- **Future Compatibility**: Supports distributed cross-daemon event broadcasting.

---

## 6. Memory Interface (`IVectorStore` & `IMemoryEngine`)

- **Purpose**: Manages semantic vector embeddings retrieval and short-term sliding context memory.
- **Owner**: `Ultron.AIEngineer` (`Knowledge` Subsystem)
- **Provider**: `VectorStore` Engine / `MemoryEngine`
- **Consumer**: `ContextRetriever` / `PromptAssembler`
- **Inputs**: `query_vector` (List[float]), `top_k` (int)
- **Outputs**: `VectorSearchResult[]` array
- **Request Structure**:
  ```json
  {
    "query_embedding": [0.012, -0.045, 0.128],
    "top_k": 3,
    "filters": { "file_extension": ".py" }
  }
  ```
- **Response Structure**:
  ```json
  {
    "results": [
      {
        "doc_id": "doc-101",
        "file_path": "backend/main.py",
        "similarity_score": 0.92,
        "text_chunk": "def main():\n    app = FastAPI()..."
      }
    ]
  }
  ```
- **Errors**: `VECTOR_DB_UNAVAILABLE`, `EMBEDDING_DIMENSION_MISMATCH`.
- **Lifecycle**: Database connection initialized at daemon startup.
- **Versioning**: `v1.0.0`.
- **Constraints**: Semantic search queries MUST execute under `< 50ms`.
- **Security Considerations**: Restricts vector queries to authorized user project paths.
- **Future Compatibility**: Supports hybrid dense-sparse (BM25 + Vector) search algorithms.

---

## 7. Plugin Interface (`IPlugin`)

- **Purpose**: Extension contract for third-party tool registration and capability hooks.
- **Owner**: `Ultron.ArchitectureLead` (`Extensions` Subsystem)
- **Provider**: Third-Party Plugin Developers
- **Consumer**: `PluginLoader` / `ToolRegistry`
- **Inputs**: `PluginContext` object
- **Outputs**: `CapabilityManifest` object
- **Request Structure**:
  ```json
  {
    "plugin_id": "ultron-git-tools",
    "version": "1.0.0",
    "entry_point": "plugin_main:register"
  }
  ```
- **Response Structure**:
  ```json
  {
    "registered_tools": ["git_status", "git_commit"],
    "required_permissions": ["Read", "Write"]
  }
  ```
- **Errors**: `PLUGIN_LOAD_FAILED`, `INCOMPATIBLE_PLUGIN_VERSION`, `UNAUTHORIZED_PRIVILEGE_REQUEST`.
- **Lifecycle**: Discovered at startup; loaded dynamically into runtime memory.
- **Versioning**: `v1.0.0`.
- **Constraints**: Plugins MUST declare required privilege scopes and pass security verification.
- **Security Considerations**: Plugins execute within sandboxed process memory boundaries.
- **Future Compatibility**: Supports WebAssembly (Wasm) isolated plugin runtimes.

---

## 8. Security Interface (`ISecurityEngine`)

- **Purpose**: Enforces non-bypassable policy evaluation and command sanitization before tool execution.
- **Owner**: `Ultron.ArchitectureLead` (`Security` Subsystem)
- **Provider**: `SecurityEngine`
- **Consumer**: `Dispatcher` (`Automation`)
- **Inputs**: `proposed_tool_call` (dict), `context` (RequestContext)
- **Outputs**: `SecurityEvaluationResult` object
- **Request Structure**:
  ```json
  {
    "tool_name": "file_write",
    "proposed_parameters": { "file_path": "/etc/passwd", "content": "root::0:0..." }
  }
  ```
- **Response Structure**:
  ```json
  {
    "status": "REJECTED",
    "policy_decision": "PATH_TRAVERSAL_RESTRICTED",
    "sanitized_parameters": {},
    "requires_user_approval": false
  }
  ```
- **Errors**: `POLICY_EVALUATION_ERROR`, `SANITY_CHECK_FAILED`.
- **Lifecycle**: Synchronous security gate evaluated before every tool dispatch.
- **Versioning**: `v1.0.0`.
- **Constraints**: Evaluation overhead MUST remain under `< 5ms`. Default to `REJECTED`.
- **Security Considerations**: Fail-closed policy rules ensure un-sanitized tools never execute.
- **Future Compatibility**: Supports dynamic policy reloading from signed policy files.

---

## 9. Configuration Interface (`IConfigManager`)

- **Purpose**: Loads, parses, and validates `.env` and `config.yaml` runtime settings.
- **Owner**: `Ultron.ArchitectureLead` (`Core` Subsystem)
- **Provider**: `ConfigManager`
- **Consumer**: All primary subsystems
- **Inputs**: `config_key` (string), `default_value` (any)
- **Outputs**: Validated configuration parameter value
- **Request Structure**:
  ```json
  {
    "key": "LLM_INFERENCE_ENDPOINT",
    "expected_type": "str"
  }
  ```
- **Response Structure**:
  ```json
  {
    "key": "LLM_INFERENCE_ENDPOINT",
    "value": "http://127.0.0.1:11434",
    "source": "ENV_FILE"
  }
  ```
- **Errors**: `CONFIG_KEY_NOT_FOUND`, `INVALID_CONFIG_TYPE`.
- **Lifecycle**: Loaded during system bootstrap; immutable during execution.
- **Versioning**: `v1.0.0`.
- **Constraints**: Configuration settings are read-only after initialization.
- **Security Considerations**: Masks sensitive API tokens and keys in log outputs.
- **Future Compatibility**: Supports hot-reloading non-critical configuration parameters.

---

## 10. Logging Interface (`IAuditLogger`)

- **Purpose**: Records immutable system security decisions, audit trails, and execution events.
- **Owner**: `Ultron.ArchitectureLead` (`Security` Subsystem)
- **Provider**: `AuditLogger`
- **Consumer**: All primary subsystems
- **Inputs**: `log_level` (enum), `trace_id` (string), `message` (string), `event_data` (dict)
- **Outputs**: Async append to structured audit log
- **Request Structure**:
  ```json
  {
    "log_level": "SECURITY_AUDIT",
    "trace_id": "trace-67890-uuid-v4",
    "event_type": "TOOL_POLICY_REJECTED",
    "message": "Attempted write to restricted path /etc/passwd",
    "timestamp": "2026-07-27T22:00:02Z"
  }
  ```
- **Response Structure**: Append confirmation (void).
- **Errors**: `AUDIT_LOG_WRITE_FAILED`, `DISK_FULL`.
- **Lifecycle**: System daemon startup to shutdown.
- **Versioning**: `v1.0.0`.
- **Constraints**: Audit log entries MUST be formatted as single-line structured JSON logs.
- **Security Considerations**: Logs MUST be stored with append-only file system permissions.
- **Future Compatibility**: Supports forwarding audit logs to local `syslog` or SIEM endpoints.

---

# Responsibilities

### Primary Responsibilities
- Specify canonical inter-module interface boundaries across all 10 contract categories.
- Guarantee strict producer/consumer contract isolation.
- Ensure all interface payloads enforce type safety and immutability rules.

### Secondary Responsibilities
- Serve as authoritative reference for integration test mocking.
- Guide static analysis rules verifying inter-module call compliance.

### Out of Scope
- Writing Python interface implementation classes or concrete functions.
- Managing database server deployments.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L6-DATA-MODELS` (`11 Data Models.md`).

### Child Of
- `DOC-ULTRON-L4-FOLDER-STRUCTURE` (`09 Folder Structure.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ARM v1.0` (Architecture Reference Model).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `09 Folder Structure.md` | Parent Document | Incoming | Provides physical layout hosting interface boundaries | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes canonical layering and interaction rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Contract Invariance**: Interface payload definitions MAY NOT be modified without formal Architecture Lead review and AVM version bump.
- **Strict Typing**: All payload fields MUST declare explicit JSON/Python data types.
- **Fail-Safe Security**: Security interfaces MUST fail-closed on any validation ambiguity.

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/09 Folder Structure.md` (Folder Structure)
- `10 - Flagship Projects/Ultron/01-Architecture/03 Request Lifecycle.md` (Request Lifecycle Spec)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Specification of Protobuf v3 schemas for binary RPC interface transport.
- Integration of compile-time interface contract validation hooks in CI/CD pipelines.
