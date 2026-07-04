# Network Stack

---

# 1. Purpose

To understand how networking responsibilities are divided into layers and how those layers cooperate to enable communication between applications across a network.

---

# 2. Central Question

Why is networking divided into layers instead of being implemented as one large system?

---

# 3. Problem Statement

Network communication requires many independent tasks including:

- Formatting application data
- Reliable delivery
- Addressing
- Routing
- Local delivery
- Physical transmission

Combining every responsibility into one system would make networking extremely difficult to build, maintain, and extend.

---

# 4. Definition

A Network Stack is a layered architecture consisting of networking protocols that cooperate to transmit and receive data between applications running on different devices.

---

# 5. Core Idea

Each layer solves one networking problem and provides services to the layer above while using services from the layer below.

---

# 6. Prerequisites

- What is a Computer Network
- Communication Flow
- Data, Bits and Signals
- Network Components
- Communication Media

---

# 7. Components

## Application Layer

Provides network services to applications.

Examples:

- HTTP
- HTTPS
- DNS
- SMTP

---

## Transport Layer

Provides communication between applications.

Examples:

- TCP
- UDP

---

## Network Layer

Provides logical addressing and routing.

Example:

- IP

---

## Data Link Layer

Provides communication inside the local network.

Example:

- Ethernet

---

## Physical Layer

Transmits physical signals.

Examples:

- Electrical Signals
- Light Pulses
- Radio Waves

---

# 8. Mechanism

1. The application generates data.
2. Each layer performs its own responsibility.
3. Each layer encapsulates the data.
4. The Physical Layer transmits signals.
5. The receiving device decapsulates the data layer by layer.
6. The destination application receives the original information.

---

# 9. Workflow

Application

↓

Application Layer

↓

Transport Layer

↓

Network Layer

↓

Data Link Layer

↓

Physical Layer

↓

Communication Medium

↓

Physical Layer

↓

Data Link Layer

↓

Network Layer

↓

Transport Layer

↓

Application Layer

↓

Application

---

# 10. Mental Model

Think of an assembly line.

Each worker performs one specialized task before passing the product to the next worker.

No worker builds the entire product alone.

---

# 11. Implementation

Modern networks primarily implement the TCP/IP protocol suite.

---

# 12. Variants / Types

- TCP/IP Model
- OSI Model

---

# 13. Examples

- Opening a webpage
- Streaming video
- SSH
- Online gaming

---

# 14. Edge Cases

- Unsupported protocol
- Missing protocol layer
- Incorrect encapsulation
- Layer failure

---

# 15. Limitations

Layered communication introduces processing overhead but greatly improves modularity and scalability.

---

# 16. Security Perspective

Different security technologies inspect different layers depending on the type of communication being analyzed.

---

# 17. Attacker Perspective

Attackers exploit weaknesses at different protocol layers.

---

# 18. Defender Perspective

Defenders monitor multiple protocol layers to detect malicious activity.

---

# 19. Investigation Perspective

Incident responders analyze network traffic layer by layer to reconstruct communication.

---

# 20. Common Misconceptions

- The Network Stack is not a single protocol.
- Layers are logical, not physical hardware.
- Every layer has one primary responsibility.

---

# 21. Related Concepts

- Protocols and Encapsulation
- OSI Model
- TCP/IP Model
- Ethernet
- IP
- TCP

---

# 22. Connections

The Network Stack provides the architectural foundation for every modern computer network.

---

# 23. Summary

The Network Stack organizes communication into specialized layers. Each layer solves one networking problem, making networks modular, scalable, and easier to maintain.

---

# 24. Review Questions

1. Why is the Network Stack layered?
2. What is the responsibility of each layer?
3. Why doesn't one layer perform every networking task?
4. What are the advantages of modular design?
5. What happens during encapsulation?

---

# 25. Keywords

Network Stack

Layer

TCP/IP

OSI

Application Layer

Transport Layer

Network Layer

Data Link Layer

Physical Layer

Encapsulation
