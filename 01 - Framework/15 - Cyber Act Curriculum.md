# Cyber Act v2 — Master Curriculum

> Status: Active
> Version: 1.0
> Last Updated:

---

# Purpose

Cyber Act is the master learning curriculum for this career system.

It defines:

- What to learn
- When to learn it
- Why it is important
- How it connects to projects
- How it produces engineering capability

Cyber Act is not a collection of notes or courses.

It is a structured engineering curriculum that transforms theoretical knowledge into practical capability through continuous implementation.

This document supersedes ICCLS.

The Healthcare V2 / Detection Engineering Platform module checklists represent the execution layer beneath this curriculum and should be mapped to the stages defined here rather than maintained as separate learning roadmaps.

---

# Curriculum Principles

The following principles govern every stage and module.

1. Attack Lifecycle is the primary learning spine.

2. Engineering knowledge is learned just-in-time through prerequisite modules rather than completing all foundations upfront.

3. Every module must contribute to one or more flagship projects.

4. Every module must produce portfolio evidence.

5. Every module must update the Skill Matrix.

6. Reading alone is never considered completion.

7. Implementation is mandatory.

8. Every stage ends with an integration review.

9. Progress is measured by engineering capability rather than content consumed.

10. The curriculum remains stable. Only module content evolves.

---

# Curriculum Structure

```text
Cyber Act
│
├── Stage 0 — Engineering Foundations Repository
├── Stage 2 — Secure Software Engineering
├── Stage 3 — Attack Lifecycle
├── Stage 4 — Detection Engineering
├── Stage 5 — Enterprise Security
├── Stage 6 — Professional Engineering
└── Final Integration
```

---

# Stage 0 — Engineering Foundations Repository

> This is **not** a sequential stage.

It serves as a repository of prerequisite knowledge that later modules reference when required.

The objective is to learn engineering concepts **just before they are needed**, rather than completing every foundation before beginning security topics.

---

## Purpose

The Engineering Foundations Repository exists to provide the minimum engineering knowledge required to understand later security concepts.

Modules are pulled on demand by later stages.

Example:

Execution
→ Processes
→ Memory
→ Windows Architecture

Credential Access
→ Authentication Internals
→ Windows Architecture
→ Logging

Secure APIs
→ HTTP
→ REST
→ Authentication Internals

---

## Foundation Areas

### Development & Tooling

- [ ] P-Git
- [ ] P-Linux
- [ ] P-Windows
- [ ] P-Python
- [ ] P-SQL
- [ ] P-HTTP / REST APIs

---

### Operating Systems

- [ ] P-Processes
- [ ] P-Threads
- [ ] P-Memory
- [ ] P-Filesystems
- [ ] P-Linux Internals
- [ ] P-Windows Internals

---

### Networking

- [ ] P-TCP/IP
- [ ] P-DNS
- [ ] P-Routing
- [ ] P-Sockets
- [ ] P-Network Troubleshooting

---

### Identity & Security

- [ ] P-Authentication Internals
- [ ] P-Authorization Concepts
- [ ] P-Cryptography Fundamentals
- [ ] P-Logging Fundamentals

---

## Repository Rules

This repository is **not** completed from top to bottom.

Instead:

1. Stage 2–6 modules declare prerequisites.
2. Learn only the required prerequisite.
3. Return immediately to the active Cyber Act module.

This keeps learning focused while avoiding unnecessary delays.

---

## Completion Criteria

A prerequisite module is considered complete when:

- ✓ Core concepts understood
- ✓ Small practical exercise completed
- ✓ Branch Knowledge updated
- ✓ Required project integration completed (if applicable)

Completion here does **not** imply mastery.

Mastery is achieved only after the concept is reinforced through later Cyber Act stages.

---

# Stage 2 — Secure Software Engineering 🔧

> **Primary Laboratory:** Secure Healthcare Platform V2

Stage 2 develops the knowledge required to design, build, and secure modern web applications.

Unlike traditional courses, every module must be implemented inside Healthcare V2 immediately after learning.

Healthcare V2 is not simply a project.

It is the practical laboratory for Secure Software Engineering.

---

## Objectives

Upon completing this stage, the learner should be able to:

- Design secure web applications.
- Build secure authentication and authorization systems.
- Defend against common web attacks.
- Perform application threat modeling.
- Develop secure APIs.
- Implement secure engineering practices.
- Explain security design decisions during interviews.

---

## Learning Philosophy

Every module follows the same execution pattern.

Prerequisites

↓

Theory

↓

Branch Knowledge

↓

Mini Practice

↓

Healthcare V2 Implementation

↓

Testing

↓

Documentation

↓

Interview Preparation

↓

Portfolio Evidence

↓

Skill Matrix Update

---

# Modules

## S2-M01 Authentication

Prerequisites

- P-Authentication Internals
- P-HTTP

Deliverables

- Secure Login
- Password Storage
- Password Reset
- Account Lockout
- Session Initialization

---

## S2-M02 Authorization

Prerequisites

- Authentication

Deliverables

- RBAC
- Permission Management
- Access Control
- Role Hierarchy

---

## S2-M03 Sessions & Cookies

Prerequisites

- HTTP
- Authentication

Deliverables

- Secure Sessions
- Cookie Security
- Session Expiration
- Session Rotation

---

## S2-M04 JWT

Prerequisites

- Sessions
- Cryptography Fundamentals

Deliverables

- JWT Authentication
- Token Validation
- Refresh Tokens

---

## S2-M05 OAuth2 & OpenID Connect

Prerequisites

- Authentication
- JWT

Deliverables

- OAuth Flows
- Identity Providers
- Token Exchange

---

## S2-M06 Multi-Factor Authentication

Prerequisites

- Authentication

Deliverables

- TOTP
- Backup Codes
- Recovery Flow

---

## S2-M07 Secure API Design

Prerequisites

- HTTP
- REST APIs

Deliverables

- REST Security
- API Authentication
- Rate Limiting
- Versioning

---

## S2-M08 Input Validation & Output Encoding

Prerequisites

- HTTP

Deliverables

- Validation Layer
- Sanitization
- Encoding

---

## S2-M09 CSRF Protection

Prerequisites

- Sessions
- Cookies

Deliverables

- CSRF Tokens
- SameSite Cookies
- Verification

---

## S2-M10 XSS Prevention

Prerequisites

- HTML
- JavaScript Basics

Deliverables

- Output Encoding
- CSP
- Sanitization

---

## S2-M11 SQL Injection Prevention

Prerequisites

- SQL

Deliverables

- Parameterized Queries
- ORM Safety
- Input Validation

---

## S2-M12 Secure File Upload

Prerequisites

- Filesystems

Deliverables

- MIME Validation
- Virus Scanning
- Storage Isolation

---

## S2-M13 Cryptography

Prerequisites

- Cryptography Fundamentals

Deliverables

- Encryption
- Hashing
- Key Management

---

## S2-M14 Secrets Management

Prerequisites

- Cryptography

Deliverables

- Environment Variables
- Secret Rotation
- Secure Configuration

---

## S2-M15 Audit Logging

Prerequisites

- Logging Fundamentals

Deliverables

- Audit Trail
- Security Events
- Immutable Logs

---

## S2-M16 Threat Modeling

Prerequisites

- Secure Architecture

Deliverables

- STRIDE
- Attack Surface
- Risk Analysis

---

## S2-M17 Security Testing

Prerequisites

- Previous Modules

Deliverables

- Functional Security Tests
- Abuse Cases
- Regression Testing

---

## S2-M18 OWASP Top 10 Review

Prerequisites

- All Previous Modules

Deliverables

- OWASP Mapping
- Security Gap Analysis
- Final Review

---

# Stage Deliverables

Upon completion of Stage 2, Healthcare V2 should contain:

- Production-quality authentication
- Robust authorization
- Secure session management
- Secure APIs
- Audit logging
- Threat model
- Security tests
- Complete documentation

---

# Stage Review

Review includes:

- Healthcare milestone verification
- Documentation review
- Interview walkthrough
- Skill Matrix update
- Portfolio evidence generation

---

# Completion Criteria

Stage 2 is complete when:

- Every module has been implemented.
- Every implementation has been tested.
- Documentation is complete.
- Healthcare V2 demonstrates production-quality secure engineering.
- The learner can confidently explain every security design decision made throughout the project.

---

# Stage 3 — Attack Lifecycle 🕵️ 🎯

> **Primary Laboratory:** Detection Engineering Platform + HTB

Stage 3 forms the core of Cyber Act.

The Attack Lifecycle serves as the primary learning spine, allowing every offensive technique to immediately translate into defensive understanding and engineering implementation.

Every attack module follows the same execution pattern:

Attack Technique

↓

Engineering Prerequisites

↓

Hands-on Lab (HTB / Practice)

↓

Detection Rule

↓

Detection Platform Integration

↓

Investigation

↓

Documentation

↓

Interview Preparation

↓

Skill Matrix Update

---

## Objectives

Upon completing this stage, the learner should be able to:

- Understand attacker behavior.
- Explain why techniques work.
- Detect each technique.
- Investigate attacks.
- Build corresponding detections.
- Map techniques to MITRE ATT&CK.
- Translate offensive knowledge into defensive engineering.

---

# Modules

## S3-M01 Reconnaissance

Prerequisites

- Networking
- DNS
- HTTP

Deliverables

- Enumeration techniques
- Passive reconnaissance
- Active reconnaissance
- Detection opportunities

---

## S3-M02 Initial Access

Prerequisites

- HTTP
- Authentication
- Secure APIs

Deliverables

- Web attacks
- Credential attacks
- Initial compromise
- Detection opportunities

---

## S3-M03 Execution

Prerequisites

- Processes
- Python
- PowerShell
- Windows Internals

Deliverables

- Process execution
- Script execution
- Command execution
- Detection rules

---

## S3-M04 Persistence

Prerequisites

- Windows Services
- Linux Services
- Registry
- Scheduled Tasks

Deliverables

- Persistence mechanisms
- Startup techniques
- Detection rules

---

## S3-M05 Privilege Escalation

Prerequisites

- File Permissions
- Operating System Internals

Deliverables

- Linux PrivEsc
- Windows PrivEsc
- Detection opportunities

---

## S3-M06 Defense Evasion

Prerequisites

- Logging
- Windows Internals

Deliverables

- Evasion techniques
- Logging bypass concepts
- Detection improvements

---

## S3-M07 Credential Access

Prerequisites

- Authentication Internals
- Windows Security

Deliverables

- Credential attacks
- Authentication abuse
- Detection rules

---

## S3-M08 Discovery

Prerequisites

- Networking
- Operating Systems

Deliverables

- Enumeration
- Host discovery
- Domain discovery
- Detection opportunities

---

## S3-M09 Lateral Movement

Prerequisites

- Networking
- Authentication
- Windows

Deliverables

- Remote execution
- Authentication abuse
- Detection rules

---

## S3-M10 Collection

Prerequisites

- File Systems

Deliverables

- Data collection
- Staging
- Detection opportunities

---

## S3-M11 Exfiltration

Prerequisites

- Networking
- HTTP
- DNS

Deliverables

- Exfiltration techniques
- Network monitoring
- Detection rules

---

## S3-M12 Command & Control

Prerequisites

- Networking
- HTTP
- DNS

Deliverables

- Beaconing
- Communication channels
- Detection opportunities

---

## S3-M13 Impact

Prerequisites

- Operating Systems
- File Systems

Deliverables

- Ransomware concepts
- Data destruction
- Detection rules

---

# Stage Deliverables

Upon completion of Stage 3:

- Every ATT&CK tactic understood.
- HTB reinforcement completed.
- Detection rule produced for every tactic.
- MITRE ATT&CK mapping completed.
- Detection Platform expanded with corresponding capabilities.
- Investigation notes completed.

---

# Stage Review

Review includes:

- Complete ATT&CK walkthrough.
- Detection Platform milestone review.
- HTB review.
- Investigation review.
- Interview walkthrough.
- Skill Matrix update.

---

# Completion Criteria

Stage 3 is complete when:

- Every attack tactic has been studied.
- Every tactic has corresponding defensive understanding.
- Every tactic has at least one implemented detection.
- Every tactic has supporting HTB practice.
- Detection Platform demonstrates end-to-end ATT&CK coverage.

---

# Stage 4 — Detection Engineering 🕵️

> **Primary Laboratory:** Detection Engineering Platform

Stage 4 transforms offensive understanding into defensive capability.

The objective is not simply to understand logs or SIEMs, but to engineer reliable detections that identify malicious activity while minimizing false positives.

Every module should produce a tangible improvement to the Detection Engineering Platform.

---

## Objectives

Upon completing this stage, the learner should be able to:

- Collect security telemetry.
- Normalize and enrich log data.
- Engineer detection rules.
- Correlate events across multiple sources.
- Map detections to MITRE ATT&CK.
- Perform threat hunting.
- Validate detections through testing.
- Investigate alerts efficiently.

---

## Learning Philosophy

Every module follows the same execution pipeline.

Prerequisites

↓

Theory

↓

Telemetry Analysis

↓

Mini Lab

↓

Detection Engineering Platform

↓

Detection Rule Development

↓

Testing

↓

Documentation

↓

Investigation

↓

Interview Preparation

↓

Skill Matrix Update

---

# Modules

## S4-M01 Windows Event Logs

Prerequisites

- Logging Fundamentals
- Windows Internals

Deliverables

- Event Log Architecture
- Security Logs
- System Logs
- Operational Logs

---

## S4-M02 Sysmon

Prerequisites

- Windows Event Logs

Deliverables

- Sysmon Configuration
- Event IDs
- Process Creation
- Network Connections
- Image Loads

---

## S4-M03 Linux Logging

Prerequisites

- Linux Internals

Deliverables

- Syslog
- journald
- auditd
- Authentication Logs

---

## S4-M04 Event Tracing for Windows (ETW)

Prerequisites

- Windows Internals

Deliverables

- ETW Providers
- ETW Collection
- ETW Security Use Cases

---

## S4-M05 Sigma Rules

Prerequisites

- Windows Logs
- Sysmon

Deliverables

- Sigma Syntax
- Rule Development
- Rule Testing
- Rule Conversion

---

## S4-M06 KQL

Priority

Interview

Deliverables

- Basic Queries
- Hunting Queries
- Detection Queries

---

## S4-M07 SPL

Priority

Interview

Deliverables

- Search Syntax
- Dashboards
- Detection Queries

---

## S4-M08 Log Normalization

Prerequisites

- Windows Logs
- Linux Logs

Deliverables

- Common Schema
- Parsing
- Field Mapping

---

## S4-M09 Event Correlation

Prerequisites

- Log Normalization

Deliverables

- Multi-Event Detection
- Correlation Engine
- Alert Logic

---

## S4-M10 IOC Matching

Prerequisites

- Threat Intelligence

Deliverables

- IOC Database
- Hash Matching
- IP Matching
- Domain Matching

---

## S4-M11 MITRE ATT&CK Mapping

Prerequisites

- Stage 3

Deliverables

- ATT&CK Mapping
- Coverage Analysis
- Detection Matrix

---

## S4-M12 Threat Hunting

Prerequisites

- Correlation
- ATT&CK

Deliverables

- Hunt Methodology
- Hunt Playbooks
- Hunt Reports

---

## S4-M13 Incident Response

Prerequisites

- Threat Hunting

Deliverables

- Triage
- Timeline Analysis
- Containment Strategy
- Evidence Collection

---

## S4-M14 Detection Validation

Prerequisites

- Detection Rules

Deliverables

- Detection Testing
- False Positive Analysis
- False Negative Analysis
- Rule Tuning

---

# Stage Deliverables

Upon completion of Stage 4:

- Functional Detection Engineering Platform.
- Log ingestion pipeline.
- Detection rule engine.
- Sigma support.
- Correlation engine.
- Investigation workflows.
- ATT&CK coverage mapping.
- Detection testing framework.
- Documentation.

---

# Stage Review

Review includes:

- Detection Platform milestone verification.
- Rule quality review.
- Investigation walkthrough.
- ATT&CK coverage assessment.
- Skill Matrix update.
- Portfolio evidence generation.

---

# Completion Criteria

Stage 4 is complete when:

- Detection Platform provides end-to-end telemetry processing.
- Detection rules are tested and documented.
- ATT&CK coverage is mapped.
- Threat hunting workflows are implemented.
- Detection quality has been validated through testing.
- The learner can confidently explain the design and implementation of the platform during interviews.

---

# Stage 5 — Enterprise Security ☁️

> **Primary Objective:** Prepare for enterprise environments and modern production infrastructure.

Stage 5 introduces the technologies, architectures, and operational practices commonly found in enterprise environments.

Unlike previous stages, the objective is familiarity and practical competence rather than building complete production systems.

These topics strengthen placement readiness and support future specialization.

---

## Objectives

Upon completing this stage, the learner should be able to:

- Understand modern enterprise infrastructure.
- Secure containerized applications.
- Understand cloud identity concepts.
- Integrate security into CI/CD pipelines.
- Explain DevSecOps principles.
- Apply infrastructure hardening techniques.
- Discuss enterprise security during interviews.

---

## Learning Philosophy

Enterprise technologies are introduced only after strong engineering and security foundations have been established.

The objective is practical understanding rather than certification-focused learning.

---

# Modules

## S5-M01 Docker & Containers

Prerequisites

- Linux
- Processes
- Filesystems

Deliverables

- Container Fundamentals
- Images
- Volumes
- Networking
- Secure Container Practices

---

## S5-M02 CI/CD

Prerequisites

- Git

Deliverables

- Build Pipelines
- Automated Testing
- Deployment Pipelines
- Secure CI/CD Concepts

---

## S5-M03 Cloud Fundamentals

Prerequisites

- Networking
- Linux

Deliverables

- Cloud Concepts
- Compute
- Storage
- Networking
- Shared Responsibility Model

---

## S5-M04 Identity & Access Management (IAM)

Prerequisites

- Authentication
- Authorization

Deliverables

- Identity Management
- Roles
- Policies
- Least Privilege

---

## S5-M05 DevSecOps

Prerequisites

- CI/CD
- Secure SDLC

Deliverables

- Security Automation
- Pipeline Security
- Dependency Scanning
- Secrets Handling

---

## S5-M06 Monitoring & Observability

Prerequisites

- Logging
- Detection Engineering

Deliverables

- Metrics
- Logs
- Traces
- Dashboards
- Alerting Concepts

---

## S5-M07 Infrastructure Hardening

Prerequisites

- Linux
- Windows

Deliverables

- System Hardening
- Baseline Configuration
- Patch Management
- Secure Defaults

---

## S5-M08 Enterprise Secrets Management

Prerequisites

- Cryptography
- Secrets Management

Deliverables

- Secret Rotation
- Vault Concepts
- Access Control
- Key Management

---

## S5-M09 Zero Trust

Prerequisites

- IAM
- Networking

Deliverables

- Zero Trust Principles
- Continuous Verification
- Least Privilege
- Micro-Segmentation

---

# Stage Deliverables

Upon completion of Stage 5:

- Enterprise security concepts understood.
- Modern infrastructure fundamentals acquired.
- Cloud and DevSecOps interview readiness achieved.
- Documentation completed.

---

# Stage Review

Review includes:

- Enterprise architecture walkthrough.
- Infrastructure security review.
- Interview readiness assessment.
- Skill Matrix update.

---

# Completion Criteria

Stage 5 is complete when:

- Core enterprise technologies are understood.
- Enterprise security concepts can be explained confidently.
- Modern infrastructure security practices are understood.
- The learner is prepared for enterprise-focused technical interviews.

---

# Stage 6 — Professional Engineering 🚀

> **Primary Objective:** Transform technical capability into professional capability.

Stage 6 focuses on engineering practices, communication, portfolio development, and interview readiness.

Technical knowledge alone is insufficient.

A professional engineer must also communicate, document, justify, and present engineering decisions effectively.

---

## Objectives

Upon completing this stage, the learner should be able to:

- Design maintainable systems.
- Produce professional documentation.
- Communicate technical ideas effectively.
- Prepare for technical interviews.
- Build a strong professional portfolio.

---

# Modules

## S6-M01 Software Architecture

Deliverables

- Layered Architecture
- Design Patterns
- Scalability
- Maintainability

---

## S6-M02 System Design

Deliverables

- High-Level Design
- Low-Level Design
- Trade-offs
- Capacity Thinking

---

## S6-M03 Documentation

Deliverables

- Technical Documentation
- Architecture Documents
- API Documentation
- Project Reports

---

## S6-M04 Testing Strategy

Deliverables

- Unit Testing
- Integration Testing
- Security Testing
- Test Planning

---

## S6-M05 Git Workflow

Deliverables

- Branch Strategy
- Pull Requests
- Code Reviews
- Release Workflow

---

## S6-M06 Technical Communication

Deliverables

- Technical Writing
- Explaining Concepts
- Design Discussions
- Documentation Quality

---

## S6-M07 Technical Presentations

Deliverables

- Project Demonstrations
- Architecture Walkthroughs
- Technical Talks

---

## S6-M08 Resume Development

Deliverables

- ATS Optimization
- Resume Versions
- Project Descriptions
- Achievement Statements

---

## S6-M09 Interview Preparation

Deliverables

- Technical Questions
- Behavioral Questions
- System Design Discussions
- Mock Interviews

---

## S6-M10 Open Source Contribution

Deliverables

- Documentation Contributions
- Bug Fixes
- Feature Contributions
- Community Collaboration

---

## S6-M11 Technical Writing

Deliverables

- Engineering Blogs
- Detection Write-ups
- Security Articles
- Research Notes

---

# Stage Deliverables

Upon completion of Stage 6:

- Professional engineering portfolio completed.
- Resume placement-ready.
- Interview preparation completed.
- Communication skills strengthened.
- Flagship projects fully documented.

---

# Final Integration

The completion of Cyber Act should result in:

- Production-quality Secure Healthcare Platform V2.
- Production-quality Detection Engineering Platform.
- Comprehensive technical documentation.
- Strong GitHub portfolio.
- Placement-ready resume.
- Interview readiness.
- Demonstrable engineering capability.

---

# Stage Review

Review includes:

- Portfolio review.
- Resume review.
- Mock technical interview.
- Mock HR interview.
- Skill Matrix review.
- Career roadmap review.

---

# Completion Criteria

Cyber Act is considered complete when:

- Every stage has been completed.
- Both flagship projects are production quality.
- The Skill Matrix demonstrates placement readiness.
- Portfolio evidence exists for every major skill.
- Resume accurately reflects engineering capability.
- The learner can confidently explain, implement, and defend the engineering decisions made throughout both flagship projects.

---

# Final Principle

Cyber Act is not a checklist of topics.

It is an engineering curriculum designed to develop the knowledge, practical skills, professional habits, and portfolio required to become an Application Security Engineer or Detection Engineer.

