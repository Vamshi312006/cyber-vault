---
tags:
  - networking
  - osi
  - layer2
  - llc
  - mac
type: concept
status: complete
---

# LLC vs MAC

## Definition

The Data Link Layer (Layer 2) is divided into two sublayers:

1. Logical Link Control (LLC)
2. Media Access Control (MAC)

Each sublayer performs a different set of responsibilities that together enable reliable communication over a local network.

---

# Why Is Layer 2 Divided?

Layer 2 has two major responsibilities:

- Providing services to the Network Layer (Layer 3).
- Managing communication over the physical medium.

Separating these responsibilities allows different Layer 2 technologies (such as Ethernet and Wi-Fi) to provide the same services to Layer 3 while implementing their own communication methods.

---

# Layer 2 Architecture

    +-----------------------------+
    | Layer 3 (Network)           |
    +-----------------------------+
    | Logical Link Control (LLC)  |
    +-----------------------------+
    | Media Access Control (MAC)  |
    +-----------------------------+
    | Layer 1 (Physical)          |
    +-----------------------------+

---

# Logical Link Control (LLC)

## Purpose

Acts as the interface between the Data Link Layer and the Network Layer.

## Responsibilities

- Communicates with Layer 3.
- Identifies which Layer 3 protocol is being carried.
- Allows multiple Layer 3 protocols to use the same data link.
- Provides logical communication services.

## Does Not Perform

- MAC Addressing
- Frame creation
- Media access
- Error detection

---

# Media Access Control (MAC)

## Purpose

Controls communication over the physical network medium.

## Responsibilities

- MAC Addressing
- Frame creation
- Frame transmission
- Medium access control
- Error detection using FCS
- Local frame delivery

## Does Not Perform

- Routing
- IP Addressing
- End-to-end communication

---

# LLC vs MAC Comparison

| Feature | LLC | MAC |
|---------|-----|-----|
| Position | Upper Layer 2 | Lower Layer 2 |
| Communicates With | Layer 3 | Layer 1 |
| Uses MAC Addresses | No | Yes |
| Creates Frames | No | Yes |
| Controls Medium | No | Yes |
| Performs Error Detection | No | Yes |

---

# IEEE Standards

| Standard | Purpose |
|----------|---------|
| IEEE 802.2 | Logical Link Control (LLC) |
| IEEE 802.3 | Ethernet |
| IEEE 802.11 | Wi-Fi |

---

# Packet Flow

    Application Data
            │
            ▼
       Layer 3 Packet
            │
            ▼
            LLC
            │
            ▼
            MAC
            │
            ▼
      Ethernet Frame
            │
            ▼
      Layer 1 (Bits)

---

# Key Takeaways

- Layer 2 is divided into LLC and MAC.
- LLC provides services to Layer 3.
- MAC manages communication over the physical medium.
- Ethernet and Wi-Fi implement different MAC sublayers while exposing similar services to higher layers.
- Most practical networking focuses on the MAC sublayer.

---

# Related Notes

- [[Layer 2 - Overview]]
- [[Why Layer 2 Exists]]
- [[Responsibilities]]
- [[Ethernet]]

