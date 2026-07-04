---
tags:
  - networking
  - osi
  - layer2
type: concept
status: complete
---

# Responsibilities

## Definition

The Data Link Layer (Layer 2) is responsible for reliable node-to-node communication within a Local Area Network (LAN). It organizes data into frames, identifies devices using MAC addresses, detects transmission errors, controls access to the communication medium, and delivers frames between directly connected devices.

---

# Responsibilities Overview

1. Framing
2. Physical Addressing
3. Error Detection
4. Media Access Control
5. Local Delivery
6. Flow Control

---

# 1. Framing

## Purpose

Convert a continuous stream of bits into structured units called **frames**.

## Why It Exists

Without framing, the receiver cannot determine:

- Where a message starts.
- Where a message ends.
- Which bits belong together.

## Process

    Bits
      │
      ▼
    Frame

---

# 2. Physical Addressing

## Purpose

Identify devices on the local network.

## Address Used

- MAC Address

## Why It Exists

When multiple devices share the same LAN, every frame must contain the source and destination MAC address so the correct device receives it.

---

# 3. Error Detection

## Purpose

Detect corruption that occurs during transmission.

## Mechanism

- Frame Check Sequence (FCS)
- Cyclic Redundancy Check (CRC)

## Result

The receiver recalculates the CRC.

- If the values match → Accept the frame.
- If the values do not match → Discard the frame.

> Ethernet detects errors but does not correct them.

---

# 4. Media Access Control

## Purpose

Control how devices access the communication medium.

## Why It Exists

If multiple devices transmit simultaneously on a shared medium, collisions or interference may occur.

## Examples

- CSMA/CD (Traditional Ethernet)
- CSMA/CA (Wi-Fi)

---

# 5. Local Delivery

## Purpose

Deliver frames between devices on the same local network.

## Scope

Layer 2 communication is limited to a single LAN.

Communication between different networks requires Layer 3 (Network Layer).

---

# 6. Flow Control

## Purpose

Prevent a fast sender from overwhelming a slower receiver.

## Note

Flow control depends on the specific Layer 2 technology and is not implemented identically across all protocols.

---

# Summary

| Responsibility | Purpose |
|---------------|---------|
| Framing | Organize bits into frames |
| Physical Addressing | Identify devices using MAC addresses |
| Error Detection | Detect corrupted frames |
| Media Access Control | Coordinate access to the communication medium |
| Local Delivery | Deliver frames within the same LAN |
| Flow Control | Regulate transmission speed |

---

# Key Takeaways

- Layer 2 communicates using frames.
- Devices are identified using MAC addresses.
- Ethernet performs error detection but not error correction.
- Layer 2 communication is limited to the local network.
- These responsibilities form the foundation of Ethernet, switching, ARP, and VLANs.

---

# Related Notes

- [[Layer 2 - Overview]]
- [[Why Layer 2 Exists]]
- [[LLC vs MAC]]
- [[Ethernet]]

