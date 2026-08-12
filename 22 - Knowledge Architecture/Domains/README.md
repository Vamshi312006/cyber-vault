---
id: "ka-doc-domains-readme"
title: "Cyber Act Knowledge Architecture - Domain Registry Guidelines"
type: "registry-guidelines"
domain: "Knowledge Architecture"
branch: "Governance & Systems Schema"
maintainer: "Cyber Act Systems Architect"
last_audited: "2026-07-29"
---

# Domain Registry Guidelines (`Domains/`)

## 1. Overview
The `Domains/` directory serves as the canonical registry for the 24 cybersecurity domains composing the Cyber Act Knowledge Architecture.

Every domain is registered as an individual markdown file adhering strictly to the **13-Attribute Domain Standard** defined in `22 - Knowledge Architecture/02 - Domain Architecture Framework.md`.

---

## 2. File Naming Standard
Domain registry files must be named using the following exact format:
`Domain-[ID]-[Name-Slug].md`

- `[ID]`: Two-digit zero-padded integer (`01` through `99`).
- `[Name-Slug]`: Lowercase kebab-case descriptive title.

### Examples:
- `Domain-01-Endpoint-Security.md`
- `Domain-02-Identity-Access-Management.md`
- `Domain-03-Network-Security.md`

---

## 3. Mandatory Domain Attributes
Every domain specification must contain all 13 required section headers:
1. `## 1. Domain Identity`
2. `## 2. Purpose`
3. `## 3. Scope`
4. `## 4. Responsibilities`
5. `## 5. Parent`
6. `## 6. Children Branches`
7. `## 7. Dependencies`
8. `## 8. Technical Inputs`
9. `## 9. Technical Outputs`
10. `## 10. Related Labs & Practical Exercises`
11. `## 11. Related Flagship Projects`
12. `## 12. Related Career Roles & Tiers`
13. `## 13. Related Attack Lifecycle Stages & ATT&CK Tactics`

---

## 4. Registration Workflow for New Domains (`Domain-25+`)
1. Obtain approval via an RFC ADR entry in `14 - Engineering Decisions`.
2. Instantiate a new domain file using the template in `02 - Domain Architecture Framework.md`.
3. Add the file link to `22 - Knowledge Architecture/06 - Master Index.md`.
