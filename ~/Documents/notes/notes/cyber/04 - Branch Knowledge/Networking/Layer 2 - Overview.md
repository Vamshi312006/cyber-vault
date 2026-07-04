---
tags:
  - networking
  - osi
  - layer2
  - datalink
type: concept
status: complete
---

# Layer 2 (Data Link Layer)

## Definition

The Data Link Layer (Layer 2) is the second layer of the OSI model responsible for **node-to-node (hop-to-hop) communication** over a local network. It organizes raw bits into frames, identifies devices using MAC addresses, detects transmission errors, and controls access to the communication medium.

---

# Why Does It Exist?

Layer 1 can only transmit raw bits over a physical medium.

It cannot determine:
- Where a message begins or ends.
- Which device should receive the data.
- Whether the data became corrupted during transmission.
- How multiple devices share the same medium.

Layer 2 solves these problems by introducing frames, MAC addressing, media access control, and error detection.

---

# Scope

Layer 2 operates **only within the local network (LAN)**.

It provides communication between directly connected devices and does not perform routing between different networks.

---

# Responsibilities

## Framing
Converts a stream of bits into structured frames.

Purpose:
- Define frame boundaries.
- Organize payload and metadata.

---

## Physical Addressing

Uses MAC addresses to identify the sender and receiver on the local network.

---

## Error Detection

Uses Frame Check Sequence (FCS) to detect corrupted frames.

Ethernet detects errors but does not correct them.

---

## Media Access Control

Controls when devices are allowed to transmit on the communication medium.

Historically prevented collisions on shared Ethernet.

---

## Local Delivery

Transfers frames between devices within the same LAN.

---

## Flow Control

Some Layer 2 technologies provide mechanisms to regulate transmission between directly connected devices.

---

# Protocol Data Unit (PDU)

| Layer | PDU |
|-------|-----|
| Layer 4 | Segment / Datagram |
| Layer 3 | Packet |
| Layer 2 | Frame |
| Layer 1 | Bits |

---

# Address Used

| Layer | Address |
|-------|---------|
| Layer 3 | IP Address |
| Layer 2 | MAC Address |

---

# Devices

- Switch
- Bridge
- Wireless Access Point (Layer 2 forwarding)

---

# Key Concepts

- Frame
- MAC Address
- Ethernet
- Switch
- ARP
- VLAN
- FCS
- MTU

---

# Related Notes

- Ethernet
- MAC Address
- Ethernet Frame
- ARP
- Switches
- VLANs
- Spanning Tree Protocol
