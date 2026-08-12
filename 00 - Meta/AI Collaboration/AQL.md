# Architecture Query Language (AQL)

> Specification Version: 1.0
> Status: Stable
> Classification: Internal Architecture Query Standard
> Scope: Architecture AI ↔ Documentation AI
> Dependencies:
> - A2A-ADP
> - AKM
> - ASL
> - AVR
> - ADS
> - ARM
> - ACP
> - AVM

---

# 1. Purpose

The Architecture Query Language (AQL) defines the standard mechanism for querying architecture knowledge.

AQL provides a deterministic interface for locating, filtering, analysing and traversing Architecture Objects.

AQL SHALL NOT modify architecture.

AQL is read-only.

---

# 2. Objectives

AQL guarantees

- deterministic architecture queries
- reproducible query results
- architecture traceability
- dependency analysis
- ownership discovery
- impact analysis

---

# 3. Query Model

Every query SHALL operate against the Architecture Graph.

```
Architecture Graph

↓

Query

↓

Resolver

↓

Validator

↓

Execution

↓

Result Set
```

---

# 4. Query Targets

AQL may query

- Architecture
- Layer
- Module
- Component
- Service
- Interface
- Workflow
- Pipeline
- Event
- Policy
- Rule
- Constraint
- Decision
- Resource
- Namespace

---

# 5. Query Categories

Supported query types

- Selection
- Filtering
- Traversal
- Relationship
- Dependency
- Ownership
- Validation
- Version
- Impact
- Statistics

---

# 6. Selection Queries

Retrieve Architecture Objects.

Examples

```
Find Module

Find Component

Find Service

Find Interface
```

Selection SHALL return matching objects only.

---

# 7. Filtering Queries

Objects may be filtered by

- Type
- Owner
- Namespace
- Status
- Version
- Layer
- Dependency
- Relationship

Multiple filters SHALL be combined deterministically.

---

# 8. Traversal Queries

Traversal SHALL navigate

- Parent
- Child
- Ancestor
- Descendant
- Dependency
- Reference
- Relationship

Traversal direction SHALL be explicit.

---

# 9. Ownership Queries

Supported ownership operations

- Find Owner
- Find Owned Objects
- Find Shared Ownership
- Find Ownership Chain

Ownership SHALL resolve uniquely.

---

# 10. Dependency Queries

Supported dependency operations

- Direct Dependencies
- Reverse Dependencies
- Dependency Tree
- Dependency Chain
- Circular Dependency Detection

Dependency traversal SHALL preserve direction.

---

# 11. Relationship Queries

Supported relationships

- contains
- owns
- depends_on
- references
- extends
- implements
- uses
- communicates_with
- validates
- orchestrates
- manages
- protects

Relationship types SHALL conform to AKM.

---

# 12. Namespace Queries

Supported namespace operations

- Find Namespace
- List Objects
- Resolve Path
- Namespace Hierarchy
- Namespace Statistics

Namespaces SHALL resolve uniquely.

---

# 13. Version Queries

Supported version operations

- Current Version
- Previous Versions
- Version History
- Compatibility
- Deprecated Objects

Version queries SHALL follow AVM.

---

# 14. Validation Queries

Supported validation operations

- Invalid Objects
- Missing Owners
- Broken References
- Invalid Dependencies
- Constraint Violations

Validation SHALL reference AVR.

---

# 15. Impact Analysis

Supported impact operations

- Change Impact
- Dependency Impact
- Owner Impact
- Interface Impact
- Version Impact

Impact analysis SHALL be deterministic.

---

# 16. Statistics Queries

Supported statistics

- Module Count
- Component Count
- Dependency Count
- Layer Count
- Interface Count
- Owner Distribution
- Version Distribution

Statistics SHALL reflect the current Architecture Version.

---

# 17. Result Model

Every query SHALL return

- Query ID
- Status
- Result Set
- Execution Time
- Architecture Version
- Object Count

---

# 18. Result Ordering

Results SHALL be deterministic.

Ordering precedence

```
Identity

↓

Hierarchy

↓

Namespace

↓

Name
```

---

# 19. Query Scope

Queries may execute against

- Entire Architecture
- Namespace
- Layer
- Module
- Component
- Version

Scope SHALL be explicitly declared.

---

# 20. Query Constraints

AQL SHALL NOT

- modify architecture
- modify ownership
- create objects
- delete objects
- change versions
- execute implementation logic

Read-only execution is mandatory.

---

# 21. Error Model

Every failed query SHALL return

- Error Code
- Query ID
- Failure Reason
- Resolution Hint

Query execution SHALL NOT terminate the architecture system.

---

# 22. Standard Error Codes

```
AQL-001 Invalid Query

AQL-002 Unknown Object

AQL-003 Unknown Namespace

AQL-004 Invalid Relationship

AQL-005 Invalid Dependency

AQL-006 Invalid Scope

AQL-007 Invalid Version

AQL-008 Broken Reference

AQL-009 Permission Denied

AQL-010 Unsupported Operation
```

---

# 23. Performance Model

Query execution SHALL be

- deterministic
- repeatable
- side-effect free
- architecture preserving

Execution SHALL NOT alter Architecture State.

---

# 24. Compliance

AQL compliance requires

- valid query structure
- valid scope
- valid object references
- valid relationships
- deterministic execution
- immutable architecture state

---

# 25. Protocol Guarantee

AQL guarantees

- deterministic architecture discovery
- complete architecture navigation
- dependency traceability
- ownership traceability
- version-aware querying
- architecture-safe analysis

AQL is the canonical query interface for every Architecture Object managed by the Architecture AI and Documentation AI.
