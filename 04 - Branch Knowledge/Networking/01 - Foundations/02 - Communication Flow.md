# Communication Flow

---

# 1. Purpose

To understand how data travels from one computer to another and identify the role of every component involved in the communication process.

---

# 2. Central Question

How does data travel from one computer to another?

---

# 3. Problem Statement

Applications create data but cannot communicate directly over a network.

A communication process is required to prepare, transmit, receive, and reconstruct data between devices.

---

# 4. Definition

Communication Flow is the sequence of operations through which data moves from a source application to a destination application across a network.

---

# 5. Core Idea

Data does not move directly from one application to another.

Instead, it passes through multiple hardware and software components, each performing a specific responsibility before the destination application receives the original data.

---

# 6. Prerequisites

- What is a Computer Network

---

# 7. Components

## User

Creates information.

---

## Application

Generates the data to be transmitted.

Examples:

- Browser
- Discord
- SSH
- FTP Client

---

## Operating System

Manages hardware resources and networking services.

---

## Network Stack

Implements networking protocols and prepares data for transmission.

---

## Network Interface Card (NIC)

Converts digital data into physical signals and converts received signals back into digital data.

---

## Communication Medium

Carries physical signals between devices.

Examples:

- Ethernet
- Fiber Optic
- Wi-Fi

---

## Destination Device

Performs the reverse communication process until the destination application receives the original data.

---

# 8. Mechanism

1. User creates information.
2. Application converts information into digital data.
3. Operating System receives the request.
4. Network Stack prepares the data.
5. NIC converts digital data into physical signals.
6. Communication medium carries the signals.
7. Destination NIC reconstructs digital data.
8. Destination Network Stack processes the data.
9. Destination Operating System delivers the data.
10. Destination Application presents the information to the user.

---

# 9. Workflow

User

↓

Application

↓

Operating System

↓

Network Stack

↓

NIC

↓

Communication Medium

↓

Destination NIC

↓

Destination Network Stack

↓

Destination Operating System

↓

Destination Application

↓

Destination User

---

# 10. Mental Model

Think of a postal delivery system.

Person

↓

Write Letter

↓

Post Office

↓

Transport

↓

Receiving Post Office

↓

Recipient

Each stage has a different responsibility.

---

# 11. Implementation

Communication flow is implemented using operating system networking APIs, protocol stacks, network interface hardware, and communication media.

Different technologies implement the same communication process.

---

# 12. Variants / Types

Communication may occur over:

- Ethernet
- Wi-Fi
- Fiber
- Cellular Networks
- Bluetooth

The communication flow remains conceptually identical.

---

# 13. Examples

- Opening a webpage
- Sending a Discord message
- Downloading a file
- SSH into a Linux server
- Streaming a YouTube video

---

# 14. Edge Cases

- Network cable disconnected.
- NIC failure.
- Protocol mismatch.
- Destination unavailable.
- Packet loss during transmission.

---

# 15. Limitations

Communication flow alone does not guarantee:

- Security
- Reliability
- Privacy
- Authentication

Additional protocols provide these features.

---

# 16. Security Perspective

Understanding communication flow is fundamental for analyzing network traffic, malware communication, packet captures, and intrusion detection.

---

# 17. Attacker Perspective

Attackers exploit different stages of communication by intercepting, modifying, spoofing, or redirecting network traffic.

---

# 18. Defender Perspective

Defenders monitor communication flows to identify malicious activity, unauthorized connections, abnormal traffic, and policy violations.

---

# 19. Investigation Perspective

Investigators reconstruct communication flows to determine:

- Source
- Destination
- Timeline
- Protocols
- Data movement

---

# 20. Common Misconceptions

- Applications do not directly communicate over the network.
- The NIC does not decide where data goes.
- The communication medium carries physical signals, not digital bits.
- The Internet is not responsible for creating communication.

---

# 21. Related Concepts

- What is a Computer Network
- Data, Bits and Signals
- Network Stack
- Network Interface Card
- Ethernet
- TCP
- IP

---

# 22. Connections

Communication Flow provides the foundation for understanding packet transmission, protocol layering, routing, switching, and application communication.

---

# 23. Summary

Communication Flow describes how information moves through software, hardware, and communication media before reaching another computer. Every component has a specific responsibility, and together they enable reliable communication between systems.

---

# 24. Review Questions

1. Why can't an application communicate directly over a network?
2. What is the role of the operating system?
3. What does the Network Stack do?
4. What is the responsibility of the NIC?
5. What does the communication medium actually carry?
6. Why is communication flow important in cybersecurity?

---

# 25. Keywords

Communication Flow

Application

Operating System

Network Stack

NIC

Communication Medium

Signal

Data

Transmission

Reception
