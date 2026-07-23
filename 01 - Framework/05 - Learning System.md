# Learning System

> Status: Active
> Version: 2.0
> Last Updated:

---

# Purpose

This document defines the standard learning methodology for every topic throughout the career system.

The objective is not simply to acquire knowledge.

The objective is to become capable of making sound engineering decisions, building secure systems, defending those decisions, and communicating them professionally.

Learning is complete only when knowledge becomes demonstrable engineering capability.

---

# Learning Philosophy

Learning should answer five questions.

1. What problem exists?

2. Why does this technology exist?

3. How does it work?

4. Why choose this solution over others?

5. Can I build, defend, and explain it?

Knowing syntax alone is not learning.

Watching videos alone is not learning.

Reading documentation alone is not learning.

Implementation without understanding is incomplete.

Understanding without implementation is incomplete.

Engineering requires both.

---

# Learning Pipeline

Every skill follows the same pipeline.

Need

↓

Big Picture

↓

Problem Statement

↓

Technology Landscape

↓

Objectives

↓

Theory

↓

Visualization

↓

Internal Working

↓

Security Perspective

↓

Industry Usage

↓

Engineering Alternatives

↓

Engineering Decision

↓

Hands-on Practice

↓

Cyber Act Module

↓

Project Implementation

↓

Testing

↓

Documentation

↓

Interview Preparation

↓

Portfolio Evidence

↓

Revision

---

---

# Knowledge Pull

## Purpose

Knowledge gaps naturally appear throughout the learning process.

Rather than interrupting the current module to study an entire dependency, pull only the minimum knowledge required to answer the current engineering question.

Once the question has been answered, immediately return to the original module.

The objective is to preserve learning momentum while preventing unnecessary rabbit holes.

---

## Workflow

Current Module

↓

Knowledge Gap Appears

↓

Define One Specific Question

↓

Identify Required Knowledge

↓

Pull Only Required Knowledge

↓

Answer Current Question

↓

Return Immediately

↓

Continue Original Module

---

## Rules

### Rule 1 — Pull by Necessity

Only pull knowledge when the current engineering question cannot be answered.

Do not study a dependency simply because it may be useful later.

---

### Rule 2 — Minimize Scope

Study only the concepts required to answer the current question.

Avoid exploring unrelated areas of the dependency.

Example

Authentication

↓

Question

How are credentials sent?

↓

Pull

- HTTP Request
- POST Method
- Request Body

↓

Return

Do not continue learning HTTP caching, HTTP/2, HTTP/3, or unrelated topics.

---

### Rule 3 — Immediate Return

Once the question has been answered, immediately return to the original module.

Do not continue exploring the dependency.

---

### Rule 4 — Preserve Context

The original engineering problem always remains the primary focus.

Dependencies exist only to support understanding.

---

### Rule 5 — Follow the Branch Rule

Every knowledge pull follows the Branch Rule.

Knowledge Pull

↓

Answer Question

↓

Return

Avoid creating additional branches unless absolutely necessary.

---

## Success Criteria

A successful Knowledge Pull should:

- Answer the current engineering question.
- Minimize unnecessary learning.
- Preserve learning momentum.
- Prevent rabbit holes.
- Return immediately to the original module.

---

## Final Principle

Knowledge should be pulled by necessity, not curiosity.

The original module always remains the primary objective.



## Foundation Terminology

Before every major module, identify and learn all prerequisite terminology required to understand the topic.

For every term, answer the following:

1. What is it?
   - What is it physically? (function, class, object, process, protocol, program, library, framework, database, etc.)

2. Why does it exist?

3. Where does it live?
   - Browser
   - Python Process
   - Operating System
   - Network
   - Database Process
   - Cloud
   - etc.

4. Who creates it?

5. Who uses it?

6. What does it receive?

7. What does it produce / return?

8. How does it work internally? (only to the required depth)

9. HC Example

The objective is to build a concrete mental model before learning the module itself.



# Stage 1 — Need Identification

Every learning session begins by answering:

- Why am I learning this?
- Which target role requires it?
- Which project will use it?
- Which engineering problem does it solve?
- Which security problem does it solve?

Learning without purpose should be avoided.

---

# Stage 2 — Big Picture

Understand where this topic belongs.

Example

Authentication

↓

Authorization

↓

Sessions

↓

Secure APIs

↓

Logging

↓

Detection

Do not study details yet.

Build a mental map first.

---

# Stage 3 — Problem Statement

Identify the original engineering problem.

Examples

HTTP is stateless.

↓

Sessions

Passwords are weak.

↓

MFA

Multiple applications require shared identity.

↓

OAuth2

Understanding the problem makes the solution easier to understand.

---

# Stage 4 — Technology Landscape

Before learning one technology, understand the ecosystem.

Example

Authentication

- Sessions
- JWT
- OAuth2
- OpenID Connect
- SAML
- API Keys
- Passkeys
- Mutual TLS

For each answer only:

- What is it?
- Why does it exist?
- When is it used?

Do not learn every alternative in depth.

Build awareness.

---

# Stage 5 — Objectives

Define measurable outcomes.

Examples

- Explain session management.
- Build secure authentication.
- Compare Sessions and JWT.
- Justify the chosen design.

Objectives should focus on capability.

---

# Stage 6 — Theory

Study:

- Concepts
- Terminology
- Architecture
- Internal mechanisms
- Design principles

Understand why the technology exists.

---

# Stage 7 — Visualization

Build mental models.

Preferred techniques

- Flowcharts
- Sequence diagrams
- State diagrams
- Architecture diagrams
- System maps

If a concept cannot be visualized, revisit the theory.

---

# Stage 8 — Internal Working

Understand the complete workflow.

Questions

- What happens first?
- Which components participate?
- What data moves?
- What state changes?
- What happens if something fails?

---

# Stage 9 — Security Perspective

Study

- Common attacks
- Misconfigurations
- Weaknesses
- Detection opportunities
- Mitigations
- Best practices

Every technology should be viewed from both attacker and defender perspectives.

---

# Stage 10 — Industry Usage

Understand where the technology is actually used.

Examples

Products

Frameworks

Cloud Platforms

Enterprise Applications

Also understand when NOT to use it.

---

# Stage 11 — Engineering Alternatives

Study competing approaches.

Examples

Sessions vs JWT

RBAC vs ABAC

REST vs GraphQL

Docker vs Podman

Compare

- Advantages
- Disadvantages
- Complexity
- Performance
- Security
- Scalability

The objective is awareness, not mastery.

---

# Stage 12 — Engineering Decision

Choose one implementation.

Document

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

Engineering is choosing the right solution—not knowing every solution.

---

# Stage 13 — Hands-on Practice

Before project integration

Build

Break

Observe

Fix

Experiment

Develop intuition.

---

# Stage 14 — Cyber Act Integration

Complete the module.

Update

- Branch Knowledge
- Skill Matrix
- Engineering Notes

---

# Stage 15 — Project Implementation

Immediately apply the knowledge.

Healthcare V2

or

Detection Engineering Platform

Implementation is the strongest proof of understanding.

---

# Stage 16 — Testing

Every implementation should be verified.

Functional Tests

Security Tests

Edge Cases

Failure Scenarios

Regression Tests

Performance

Verification is mandatory.

---

# Stage 17 — Documentation

Document

- Concept
- Architecture
- Decisions
- Trade-offs
- Lessons Learned
- References

Documentation preserves engineering knowledge.

---

# Stage 18 — Interview Preparation

Every module should answer

- What problem does it solve?
- How does it work?
- Why this solution?
- Why not another solution?
- Security implications?
- Trade-offs?
- Failure cases?
- Industry usage?

---

# Stage 19 — Portfolio Evidence

Every completed module should generate evidence.

Examples

- Healthcare feature
- Detection feature
- GitHub commit
- Documentation
- Detection rule
- Investigation
- Resume bullet
- Interview story

Knowledge without evidence is incomplete.

---

# Stage 20 — Revision

Revisit important topics periodically.

Prioritize

- Weak concepts
- Interview topics
- Frequently used skills
- Forgotten material
- Engineering decisions

Knowledge decays without reinforcement.

---

# Completion Criteria

A topic is complete only when all are true.

✓ Understand the problem

✓ Understand the ecosystem

✓ Explain the concept

✓ Explain the internals

✓ Compare alternatives

✓ Justify the chosen solution

✓ Implement it

✓ Test it

✓ Document it

✓ Defend design decisions

✓ Demonstrate it in a project

✓ Discuss confidently during interviews

---

# Final Principle

The objective is not to learn technologies.

The objective is to become an engineer capable of understanding problems, evaluating solutions, making sound engineering decisions, building secure systems, and continuously improving them through practical experience.

