---
tags:
  - networking
  - osi
  - layer2
  - encapsulation
type: concept
status: complete
---

# Encapsulation

## Definition

Encapsulation is the process of adding protocol-specific headers (and sometimes trailers) to data as it moves down the OSI model before transmission.

Each layer adds its own information required to perform its function.

---

# Purpose

Encapsulation allows every OSI layer to perform its responsibilities independently while passing data to the next lower layer.

---

# OSI Encapsulation

    Application Data
            │
            ▼
    Transport Layer
    +----------------+
    | TCP/UDP Header |
    +----------------+
            │
            ▼
    Network Layer
    +----------------+
    | IP Header      |
    +----------------+
            │
            ▼
    Data Link Layer
    +------------------------------+
    | Ethernet Header | FCS        |
    +------------------------------+
            │
            ▼
    Physical Layer
            │
            ▼
          Bits

---

# Protocol Data Units (PDU)

| Layer | PDU |
|------|------|
| Application | Data |
| Transport | Segment / Datagram |
| Network | Packet |
| Data Link | Frame |
| Physical | Bits |

---

# Layer 2 Encapsulation

At Layer 2:

- Receives an IP packet from Layer 3.
- Adds an Ethernet header.
- Adds a Frame Check Sequence (FCS).
- Creates an Ethernet Frame.
- Sends the frame to Layer 1.

---

# Decapsulation

The receiving device performs the reverse process.

    Bits
      │
      ▼
    Frame
      │
      ▼
    Packet
      │
      ▼
    Segment
      │
      ▼
      Data

Each layer removes its own header before passing the remaining data upward.

---

# Example

Suppose a browser sends an HTTP request.

Application

    GET /index.html

↓

Transport

    TCP Header + Data

↓

Network

    IP Header + TCP Segment

↓

Data Link

    Ethernet Header + IP Packet + FCS

↓

Physical

    Binary Signals

---

# Why Encapsulation Is Important

- Separates responsibilities between layers.
- Enables interoperability.
- Simplifies protocol design.
- Allows different technologies to work together.
- Makes troubleshooting easier.

---

# Security Perspective

During packet analysis (Wireshark), analysts inspect each encapsulation layer separately.

Attackers may also hide malicious traffic by abusing encapsulation techniques such as tunneling or nested protocols.

---

# Key Takeaways

- Encapsulation adds protocol information at each OSI layer.
- Layer 2 encapsulates Layer 3 packets into Ethernet frames.
- The receiving device performs decapsulation.
- Encapsulation is fundamental to all network communication.

---

# Related Notes

- [[Ethernet]]
- [[Ethernet Frame]]
- [[MTU]]
- [[Layer 3]]

