# Project Architecture

> Module ID: S2-M00
> Stage: Stage 2 - Secure Software Engineering
> Status: In Progress

---

# Purpose

Understand why software architecture exists, what engineering problems it solves, how different architectural styles compare, and why Healthcare V2 uses its chosen architecture.

---

# Learning Objectives

- [ ] Understand why software architecture exists.
- [ ] Identify the problems architecture solves.
- [ ] Compare common architectural styles.
- [ ] Understand the architecture of Healthcare V2.
- [ ] Justify architectural decisions.

---

# Lesson 1 — Why Software Architecture Exists

## Problem

As software grows, complexity grows faster than functionality.

Without architecture:

- Code becomes tightly coupled.
- Small changes break unrelated features.
- Maintenance becomes difficult.
- Testing becomes harder.
- Security becomes inconsistent.

Architecture exists to manage complexity.

---

## Engineering Goals

A good architecture should improve:

- Maintainability
- Scalability
- Security
- Readability
- Testability
- Reusability
- Extensibility
- Reliability

---

## Hospital Analogy

A hospital separates responsibilities.

Reception
↓

Doctors
↓

Laboratory
↓

Pharmacy
↓

Billing
↓

Administration

Each department has a single responsibility and communicates through defined processes.

Software architecture follows the same principle.

---

## First Principle

Architecture is **not folders**.

Architecture is the organization of responsibilities and the communication between components.

Folders are only one representation of that design.

---

## Engineering Definition

A good software architecture answers six questions:

1. What components exist?
2. What is each component responsible for?
3. Who can communicate with whom?
4. How does data flow?
5. How are changes isolated?
6. How can the system grow without becoming difficult to maintain?

---

## Healthcare V2 Connection

Current major components:

- Configuration
- Database
- Models
- Services
- Blueprints
- Templates
- Authentication
- Logging
- Security Components

The important question is not *what* these components are, but *why* they are separate.

---

## Key Takeaway

Software architecture exists to manage complexity by separating responsibilities and controlling dependencies so that a system remains maintainable, secure, and scalable.

---

# Notes

