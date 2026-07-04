# OSI Model

---

# 1. Purpose

To provide a standard conceptual model for understanding, designing, and discussing network communication by dividing it into independent layers.

---

# 2. Central Question

How can network communication be organized into clearly defined responsibilities?

---

# 3. Problem Statement

Before standardized networking models, different vendors designed incompatible networking systems.

A common reference model was needed to describe networking concepts consistently, independent of specific technologies.

---

# 4. Definition

The Open Systems Interconnection (OSI) Model is a seven-layer reference model developed by ISO that organizes network communication into logical layers, each responsible for a specific function.

The OSI Model describes how communication should be structured but does not define specific protocols.

---

# 5. Core Idea

Complex communication becomes easier to understand, implement, troubleshoot, and improve when divided into independent layers with well-defined responsibilities.

---

# 6. Prerequisites

- What is a Computer Network
- Communication Flow
- Data, Bits and Signals
- Network Components
- Communication Media
- Network Stack
- Protocols and Encapsulation

---

# 7. Components

1. Physical Layer
2. Data Link Layer
3. Network Layer
4. Transport Layer
5. Session Layer
6. Presentation Layer
7. Application Layer

---

# 8. Mechanism

Data moves downward through the seven layers at the sender.

Each layer performs its responsibility and encapsulates the data.

The receiver processes the data upward by removing protocol information layer by layer.

---

# 9. Workflow

Application

↓

Presentation

↓

Session

↓

Transport

↓

Network

↓

Data Link

↓

Physical

↓

Communication Medium

↓

Physical

↓

Data Link

↓

Network

↓

Transport

↓

Session

↓

Presentation

↓

Application

---

# 10. Mental Model

Think of constructing a building.

Each floor has a different purpose.

Together they create one complete structure.

---

# 11. Implementation

The OSI Model is primarily used as a conceptual and educational reference.

Modern networks primarily implement the TCP/IP protocol suite rather than the OSI Model directly.

---

# 12. Variants / Types

Reference Models

- OSI Model
- TCP/IP Model

---

# 13. Examples

Used for:

- Learning networking
- Troubleshooting
- Protocol classification
- Network analysis

---

# 14. Edge Cases

Some modern protocols span multiple OSI layers.

The model is conceptual rather than a strict implementation.

---

# 15. Limitations

The OSI Model is not implemented exactly in modern operating systems or Internet protocols.

---

# 16. Security Perspective

Security technologies operate at different OSI layers depending on the threats they address.

---

# 17. Attacker Perspective

Different attacks target different OSI layers.

---

# 18. Defender Perspective

Security monitoring is often organized according to OSI layers.

---

# 19. Investigation Perspective

Investigators identify the affected OSI layer to narrow the scope of troubleshooting and incident response.

---

# 20. Common Misconceptions

- The OSI Model is not the Internet.
- The OSI Model does not define protocols.
- Every protocol does not belong exclusively to one layer.

---

# 21. Related Concepts

- Network Stack
- TCP/IP Model
- Encapsulation

---

# 22. Connections

The OSI Model provides a universal language for discussing networking concepts.

---

# 23. Summary

The OSI Model is a seven-layer conceptual framework that organizes networking responsibilities into logical layers, making communication easier to understand and analyze.

---

# 24. Review Questions

1. Why was the OSI Model created?
2. Is the OSI Model actually implemented?
3. What is the purpose of each layer?
4. How does the OSI Model differ from TCP/IP?

---

# 25. Keywords

OSI

ISO

Reference Model

Seven Layers

Layered Architecture

Encapsulation

Networking
