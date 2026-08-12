---
id: "ka-doc-02"
title: "Cyber Act Knowledge Architecture - Domain Architecture Framework"
type: "domain-framework"
domain: "Knowledge Architecture"
branch: "Governance & Systems Schema"
maintainer: "Cyber Act Systems Architect"
last_audited: "2026-07-29"
---

# Domain Architecture Framework

## 1. Purpose
The **Domain Architecture Framework** establishes the standardized structural specification, metadata requirements, and registry model for the 24 cybersecurity domains composing the Cyber Act Knowledge Architecture.

It provides a uniform, highly structured template that defines domain identity, boundary scope, responsibilities, inputs, outputs, and cross-repository linkages, ensuring that all domains maintain architectural parity and machine readability.

---

## 2. Scope
- Structural specification for the 24 core cybersecurity domains.
- Definition of the 13 mandatory domain attributes.
- Extensibility guidelines for registering new domains (`Domain-25+`).
- Standardized directory layout for `22 - Knowledge Architecture/Domains/`.

---

## 3. The 13-Attribute Domain Standard

Every domain specification file registered under `22 - Knowledge Architecture/Domains/` must strictly implement the following 13 architectural attributes:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    THE 13-ATTRIBUTE DOMAIN SCHEMA                       │
└─────────────────────────────────────────────────────────────────────────┘
  1. DOMAIN IDENTITY     : Unique ID, Name, Version, Maintainer
  2. PURPOSE             : Technical rationale for domain existence
  3. SCOPE               : Explicit inclusions & boundary exclusions
  4. RESPONSIBILITIES    : Core security outcomes & functional goals
  5. PARENT              : Parent node (Layer 1: Knowledge Universe)
  6. CHILDREN            : Sub-domains, branches & specialized tracks
  7. DEPENDENCIES        : Prerequisite Domain IDs (DAG Edges)
  8. INPUTS              : Protocols, telemetry formats, standards consumed
  9. OUTPUTS             : Security models, rules, hardened configs produced
  10. RELATED LABS       : Mapped practical exercises in 05-Labs / 12-HTB_THM
  11. RELATED PROJECTS   : Mapped codebases in 10-Flagship Projects
  12. RELATED CAREER     : Mapped target roles in 13-Career System
  13. RELATED ATTACK     : Mapped stages in 03-Attack Lifecycle & ATT&CK
```

---

## 4. Domain Registry Specification & Directory Layout

Domains are stored as self-contained markdown files within the `Domains/` sub-directory:

```
22 - Knowledge Architecture/
└── Domains/
    ├── README.md                           (Registry Guidelines)
    ├── Domain-01-Endpoint-Security.md
    ├── Domain-02-Identity-Access-Management.md
    ├── Domain-03-Network-Security.md
    ├── ...
    └── Domain-24-Cloud-Security.md
```

### Domain Naming Format:
`Domain-[ID]-[Name-Slug].md`
- `[ID]`: Two-digit zero-padded integer (`01` to `99`).
- `[Name-Slug]`: Lowercase kebab-case descriptive title.

---

## 5. Standard Domain Template

Below is the authoritative, production-ready template that all domain specification files in `Domains/` must adhere to:

```markdown
---
id: "domain-[id-number]"
title: "Domain [ID]: [Domain Name]"
type: "domain-specification"
domain_id: "[ID]"
maintainer: "Cyber Act Systems Architect"
last_audited: "YYYY-MM-DD"
---

# Domain [ID]: [Domain Name]

## 1. Domain Identity
- **Domain ID**: `Domain-[ID]`
- **Canonical Name**: [Domain Name]
- **Ontology Parent**: `KU-CYBER`
- **Domain Status**: `Active` (Active / Draft / Deprecated)

## 2. Purpose
[High-level rationale explaining why this domain exists within cybersecurity engineering.]

## 3. Scope
### In Scope:
- [Item 1]
- [Item 2]

### Out of Scope:
- [Item 1 (Explicit boundary statement)]

## 4. Responsibilities
1. [Core security outcome or functional goal 1]
2. [Core security outcome or functional goal 2]

## 5. Parent
- `KU-CYBER` (Layer 1: Knowledge Universe)

## 6. Children Branches
- `[branch-id-1]` ([Branch Title in 04 - Branch Knowledge])
- `[branch-id-2]` ([Branch Title in 04 - Branch Knowledge])

## 7. Dependencies
- `Domain-[XX]` ([Prerequisite Domain Name])

## 8. Technical Inputs
- **Telemetry & Logs**: [e.g., Windows Event Logs, Sysmon, Auditd]
- **Protocols & RFCs**: [e.g., Kerberos v5, TLS 1.3, OAuth 2.0]
- **Standards**: [e.g., NIST SP 800-53, ISO 27001]

## 9. Technical Outputs
- **Architecture Models**: [e.g., Hardened Bastion Host Blueprint]
- **Detection Queries**: [e.g., Sigma rules for Process Injection]
- **Security Configurations**: [e.g., AppLocker Policy XML]

## 10. Related Labs & Practical Exercises
- `05 - Labs/[Tactic]/[Lab-Name]`
- `12 - HTB_THM/[Tactic]/[Machine-Name]`

## 11. Related Flagship Projects
- `10 - Flagship Projects/[Project-Name]`

## 12. Related Career Roles & Tiers
- `13 - Career System/06 - Placement/[Role-Name]` (e.g., Detection Engineer L2)

## 13. Related Attack Lifecycle Stages & ATT&CK Tactics
- `03 - Attack Lifecycle/Stage [X] - [Stage Name]`
- MITRE ATT&CK Tactics: `TA0001` (Initial Access), `TA0004` (Privilege Escalation)
```

---

## 6. Domain Extensibility & Addition Procedure

When adding a new domain (`Domain-25+`):
1. **Allocate Unique ID**: Select the next available 2-digit integer (`Domain-25`).
2. **Verify Boundaries**: Check existing domain scopes to ensure no overlapping responsibilities.
3. **Instantiate Schema**: Copy the Standard Domain Template into `22 - Knowledge Architecture/Domains/Domain-25-[Name].md`.
4. **Register in Master Index**: Update `22 - Knowledge Architecture/06 - Master Index.md` and `Domains/README.md`.
5. **Update Dependency Graph**: Add any directional dependencies to `03 - Knowledge Graph & Mapping Standard.md`.

---

## 7. Integration with Repository Sections

- **`04 - Branch Knowledge`**: Every folder in `04 - Branch Knowledge` maps to one or more `Children Branches` in a Domain file.
- **`06 - Detections` & `07 - Investigations`**: Rule categories reference `Domain ID` in their frontmatter.
- **`10 - Flagship Projects`**: Codebase design docs cite supported `Domain IDs`.
- **`13 - Career System`**: Role definitions map required domain competencies using `Domain IDs`.

---

## 8. AI Ingestion & Navigation Rules

For Ultron:
1. **Domain Indexing**: Ultron parses all files matching `Domains/Domain-*.md` to build the top-level domain nodes of the knowledge graph.
2. **Metadata Consistency**: Ultron validates that every domain file contains all 13 mandatory section headers.
3. **Dependency Traversal**: Ultron uses Section 7 (`Dependencies`) to enforce prerequisite checks when generating automated learning roadmaps.

---

## 9. Validation Checklist
- [x] All 13 mandatory domain attributes explicitly defined.
- [x] Production-ready domain markdown template included.
- [x] Directory layout for `22 - Knowledge Architecture/Domains/` specified.
- [x] Clear domain addition/extensibility protocol established.
- [x] Integration interfaces mapped to repository sub-systems.
