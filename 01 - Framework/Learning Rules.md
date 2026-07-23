# Learning Rules

## Purpose

Every technical topic in this vault follows the same engineering learning methodology.

The objective is not simply to understand how a technology works.

The objective is to understand:

- Why it exists.
- What problem it solves.
- What alternatives exist.
- Why one solution is chosen over another.
- How it is implemented.
- How it is attacked.
- How it is defended.

Every topic should ultimately improve engineering decision-making.

---

# Learning Pipeline

Problem

↓

Big Picture

↓

Technology Landscape

↓

Functional Understanding

↓

Practical Observation

↓

Internal Mechanism

↓

Security Perspective

↓

Engineering Alternatives

↓

Engineering Decision

↓

Implementation

↓

Testing

↓

Documentation

↓

Interview Readiness

---

# Phase 1 — Problem

Before studying any technology answer:

- What problem existed?
- Why was this technology created?
- What engineering challenge does it solve?
- What security challenge does it solve?

Never memorize solutions before understanding the problem.

---

# Phase 2 — Big Picture

Understand where the technology fits.

Example

Authentication

↓

Authorization

↓

Sessions

↓

APIs

↓

Logging

↓

Detection

Do not study details.

Build the mental map first.

---

# Phase 3 — Technology Landscape

Identify the major competing solutions.

Example

Authentication

- Sessions
- JWT
- OAuth2
- OpenID Connect
- Passkeys
- API Keys
- SAML
- Mutual TLS

For each answer only:

- What is it?
- Why does it exist?
- When is it used?

Do not attempt mastery here.

Build awareness.

---

# Phase 4 — Functional Model

Answer

- What is it?
- What does it do?
- Who uses it?
- Why is it useful?
- What are its components?
- What are its limitations?

Goal

Understand the technology without learning internals.

---

# Phase 5 — Practical Observation

Observe the technology.

Activities

- Configure it.
- Run it.
- Capture packets.
- Observe logs.
- Break it.
- Restore it.

Questions

- What enters?
- What leaves?
- What changes?

Goal

Develop intuition.

---

# Phase 6 — Internal Mechanism

Now understand

- Step-by-step workflow
- Components involved
- Protocols
- Data flow
- State changes
- Failure scenarios

Questions

- What happens internally?
- Why was it designed this way?
- What happens if something fails?

Goal

Understand implementation.

---

# Phase 7 — Security Perspective

For every technology identify

Common attacks

Common vulnerabilities

Common mistakes

Misconfigurations

Indicators of compromise

Detection opportunities

Mitigations

Best practices

Goal

Think like both attacker and defender.

---

# Phase 8 — Engineering Alternatives

Compare competing approaches.

Examples

Sessions vs JWT

RBAC vs ABAC

REST vs GraphQL

Docker vs Podman

PostgreSQL vs MySQL

Compare

- Advantages
- Disadvantages
- Complexity
- Performance
- Security
- Scalability
- Typical use cases

Goal

Develop engineering judgement.

---

# Phase 9 — Engineering Decision

For every implementation document

Problem

↓

Available Solutions

↓

Chosen Solution

↓

Reason

↓

Trade-offs

↓

Future Alternatives

If you cannot justify your decision, continue learning.

Goal

Become capable of defending engineering decisions.

---

# Phase 10 — Practical Reinforcement

Activities

Build

Break

Observe

Fix

Repeat

Explain every step.

Verify observations match the theory.

Goal

Convert understanding into intuition.

---

# Phase 11 — Documentation

Every completed topic should include

- Problem
- Big Picture
- Technology Landscape
- Functional Model
- Internal Mechanism
- Security Perspective
- Alternatives
- Engineering Decision
- Commands
- Labs
- References
- Lessons Learned

Goal

Preserve engineering knowledge.

---

# Phase 12 — Interview Readiness

Every topic should answer

What problem does it solve?

How does it work?

Why this solution?

Why not another solution?

What are the trade-offs?

Where is it used?

What are common mistakes?

How is it attacked?

How is it defended?

Goal

Be able to explain and defend every implementation.

---

# Completion Criteria

A topic is complete only when

✓ The problem is understood.

✓ The ecosystem is understood.

✓ The functional model is clear.

✓ The internal mechanism is understood.

✓ Practical observation is completed.

✓ Security perspective is covered.

✓ Alternatives are compared.

✓ Engineering decision is justified.

✓ Practical implementation is completed.

✓ Documentation is complete.

✓ Interview questions can be answered.

---

# Final Principle

Technology should never be learned in isolation.

Always learn:

Problem

↓

Available Solutions

↓

Chosen Solution

↓

Implementation

↓

Defense

↓

Engineering Decision

The objective is not to memorize technologies.

The objective is to become an engineer capable of selecting, implementing, securing, and defending the right solution for a given problem.

