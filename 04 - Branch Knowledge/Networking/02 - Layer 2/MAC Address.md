---
tags:
  - networking
  - osi
  - layer2
  - mac
type: concept
status: complete
---

# MAC Address

## Definition

A MAC (Media Access Control) Address is a unique hardware identifier assigned to a Network Interface Card (NIC). It is used by Layer 2 to identify devices within the same Local Area Network (LAN).

---

# Purpose

A MAC address enables devices on the same network to identify and communicate with one another.

Without MAC addresses, switches would not know where to forward Ethernet frames.

---

# Characteristics

- Operates at Layer 2 (Data Link Layer)
- Used only within the local network
- Typically assigned by the manufacturer
- 48 bits (6 Bytes) long
- Represented in hexadecimal

---

# Format

Example:

    00:1A:2B:3C:4D:5E

Each pair represents one byte.

| Byte | Value |
|------|-------|
| 1 | 00 |
| 2 | 1A |
| 3 | 2B |
| 4 | 3C |
| 5 | 4D |
| 6 | 5E |

---

# Structure

A MAC address consists of two parts.

| Part | Size | Purpose |
|------|------|---------|
| OUI (Organizationally Unique Identifier) | 24 bits | Identifies the manufacturer |
| Device Identifier | 24 bits | Unique identifier assigned by the manufacturer |

Example:

    00:1A:2B : 3C:4D:5E
    └─ OUI ─┘ └ Device ID ┘

---

# Types of MAC Addresses

## Unicast

Identifies a single device.

Example:

    00:1A:2B:3C:4D:5E

---

## Broadcast

Sent to every device on the LAN.

Address:

    FF:FF:FF:FF:FF:FF

---

## Multicast

Sent to a specific group of devices.

Example:

    01:00:5E:xx:xx:xx

---

# How It Is Used

When a device sends an Ethernet frame:

1. Source MAC is added.
2. Destination MAC is added.
3. The switch reads the destination MAC.
4. The frame is forwarded to the correct device.

---

# MAC Address vs IP Address

| Feature | MAC Address | IP Address |
|---------|-------------|------------|
| Layer | Layer 2 | Layer 3 |
| Purpose | Local identification | Network identification |
| Scope | Local LAN | Across networks |
| Assigned By | Manufacturer (usually) | Network administrator / DHCP |
| Changes | Rarely | Can change |

---

# Example Communication

    Laptop
       │
       ▼
Source MAC:      AA:AA:AA:AA:AA:AA
Destination MAC: BB:BB:BB:BB:BB:BB
       │
       ▼
     Switch
       │
       ▼
    Desktop

---

# Key Takeaways

- A MAC address uniquely identifies a network interface.
- It operates at Layer 2.
- Ethernet frames always contain source and destination MAC addresses.
- Switches forward frames based on MAC addresses.
- MAC addresses are only meaningful within a local network.

---

# Related Notes

- [[Ethernet]]
- [[Ethernet Frame]]
- [[OUI]]
- [[Switches]]
- [[ARP]]

