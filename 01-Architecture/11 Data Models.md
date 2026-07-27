# 11 Data Models

> Document ID: DOC-ULTRON-L6-DATA-MODELS
> Document Name: 11 Data Models
> Version: 1.0.0
> Status: Frozen
> Owner: Ultron.ArchitectureLead
> Architecture Version: 1.0.0
> Abstraction Level: L6 Data Models
> Parent Document: DOC-ULTRON-L6-API-CONTRACTS
> Child Documents: DOC-ULTRON-L5-DEPLOYMENT-ARCHITECTURE
> Dependencies: A2A-ADP v1.0, AKM v1.0, ARM v1.0, ADS v1.0
> Last Updated: 2026-07-27

---

# Purpose

This document specifies the internal domain data entities, state schemas, and persistence models governing Project Ultron.

It defines entity relationships, field types, primary keys, and storage structures for session state, vector embeddings indices, audit logs, and Tool Registry definitions.

---

# Scope

### Included in Scope
- Core domain entities (`SessionState`, `VectorDocument`, `ToolDefinition`, `AuditRecord`).
- Data entity field definitions, types, and constraints.
- Storage mapping definitions (Key-Value, Vector Store, Audit Append Log).

### Excluded from Scope
- Database vendor installation scripts or SQL driver C extensions.
- Low-level memory pointer offsets.

---

# Engineering Question

**What domain data entities, state schemas, and persistence models define the internal state of Project Ultron?**

---

# Context

This document occupies abstraction level **L6 Data Models** in the Ultron architecture hierarchy.

It derives from `10 API Contracts.md` and provides data entity specifications for `12 Deployment Architecture.md`.

```
10 API Contracts.md (L6 API Contracts)
↓
11 Data Models.md (L6 Data Models - This Document)
↓
12 Deployment Architecture.md (L5 Deployment Architecture)
```

---

# Architecture

The internal domain data entities of Ultron are structured across four core state models:

### 1. SessionState Entity (Knowledge Subsystem)
- `session_id`: String (UUID v4, Primary Key)
- `created_at`: Datetime (ISO-8601 UTC)
- `active_context_window`: List[ChatMessage]
- `working_memory_kv`: Map[String, String]
- `last_accessed`: Datetime

### 2. VectorDocument Entity (Knowledge Subsystem - RAG Index)
- `doc_id`: String (SHA-256 Hash, Primary Key)
- `file_path`: String (Canonical workspace path)
- `chunk_index`: Integer
- `content_text`: String
- `vector_embedding`: List[Float] (Dimensions: 384 / 768 / 1536)
- `indexed_at`: Datetime

### 3. ToolDefinition Entity (Development Subsystem - Tool Registry)
- `tool_name`: String (Unique Primary Key)
- `description`: String
- `required_privilege`: Enum (`Read`, `Write`, `Execute`, `Elevated`)
- `parameter_schema`: Map (JSON Schema Draft-07)
- `execution_binary`: String (Path to host executable)

### 4. AuditRecord Entity (Security Subsystem)
- `audit_id`: String (UUID v4, Primary Key)
- `request_id`: String (Foreign Key)
- `trace_id`: String (Correlation Key)
- `timestamp`: Datetime
- `action_type`: String (`TOOL_EXECUTION`, `POLICY_EVALUATION`, `USER_APPROVAL`)
- `policy_result`: Enum (`APPROVED`, `REJECTED`, `USER_DENIED`)
- `detail_json`: String

---

# Responsibilities

### Primary Responsibilities
- Specify formal data entity schemas and field constraints for Ultron.
- Ensure entity definitions maintain single responsibility and normalization.
- Align data model types with API contracts (`10 API Contracts.md`).

### Secondary Responsibilities
- Guide database schema migrations and Pydantic model implementations.
- Provide data entity references for unit test fixtures.

### Out of Scope
- Writing SQL migration scripts.
- Managing disk partition formatting.

---

# Relationships

### Parent Of
- `DOC-ULTRON-L5-DEPLOYMENT-ARCHITECTURE` (`12 Deployment Architecture.md`).

### Child Of
- `DOC-ULTRON-L6-API-CONTRACTS` (`10 API Contracts.md`).

### References
- `AKM v1.0` (Architecture Knowledge Model Ontology).
- `ADS v1.0` (Architecture Document Schema).

---

# Dependencies

| Target | Type | Direction | Reason | Criticality |
| :--- | :--- | :--- | :--- | :--- |
| `10 API Contracts.md` | Parent Document | Incoming | Provides payload schemas driving data entity definitions | Mandatory |
| `ARM v1.0` | Reference Model | Incoming | Establishes data entity boundary rules | Mandatory |
| `A2A-ADP v1.0` | Protocol | Operational | Enforces delegation and compilation rules | Mandatory |

---

# Constraints

- **Primary Key Uniqueness**: Every persisted entity must possess a unique primary identifier (`session_id`, `doc_id`, `tool_name`, `audit_id`).
- **Audit Immutability**: `AuditRecord` entities must be append-only and immutable.
- **Type Safety**: All fields must declare explicit data types (String, Integer, Datetime, Enum).

---

# References

- `10 - Flagship Projects/Ultron/01-Architecture/10 API Contracts.md` (API Contracts)
- `10 - Flagship Projects/Ultron/01-Architecture/09 Folder Structure.md` (Folder Structure)
- `00 - Meta/AI Collaboration/ADS.md` (Architecture Document Schema)

---

# Future Scope

- Specification of distributed database schemas (e.g., PostgreSQL / Redis) for enterprise multi-node deployments.
- Definition of encrypted storage fields for sensitive API keys.
