---
tags:
  - networking
  - osi
  - layer2
  - mac
  - oui
type: concept
status: complete
---

# OUI (Organizationally Unique Identifier)

## Definition

An Organizationally Unique Identifier (OUI) is the first 24 bits (3 bytes) of a MAC address. It uniquely identifies the manufacturer or organization that produced the network interface.

The OUI is assigned by the IEEE (Institute of Electrical and Electronics Engineers).

---

# Purpose

The OUI allows devices and administrators to determine the manufacturer of a Network Interface Card (NIC).

---

# MAC Address Structure

    00:1A:2B : 3C:4D:5E
    └─ OUI ─┘ └ Device ID ┘

- OUI: First 24 bits
- Device Identifier: Last 24 bits

---

# Example

MAC Address

    00:1A:2B:3C:4D:5E

Breakdown

| Portion | Value | Purpose |
|---------|-------|---------|
| OUI | 00:1A:2B | Manufacturer |
| Device Identifier | 3C:4D:5E | Unique device assigned by manufacturer |

---

# Who Assigns OUIs?

The IEEE maintains a global registry.

Manufacturers purchase an OUI from IEEE before producing network devices.

Examples include:

- Intel
- Cisco
- Dell
- HP
- TP-Link
- Apple
- Samsung

---

# Why Is OUI Important?

- Identifies hardware manufacturers.
- Helps troubleshoot network issues.
- Assists forensic investigations.
- Detects unknown or suspicious devices.
- Used by security monitoring tools.

---

# Security Perspective

Attackers can change (spoof) MAC addresses.

Although the OUI may indicate a particular vendor, it cannot be trusted as proof of the actual hardware manufacturer.

Example:

A Linux system may spoof an Apple MAC address to bypass MAC-based filtering.

---

# How to View an OUI

## Linux

    ip link

or

    ip addr

---

## Windows

    ipconfig /all

or

    getmac

---

## Lookup

Extract the first three bytes of the MAC address and compare them with the IEEE OUI database.

---

# Key Takeaways

- OUI is the first 24 bits of a MAC address.
- It identifies the manufacturer of the network interface.
- IEEE assigns OUIs globally.
- The remaining 24 bits uniquely identify the device.
- OUIs are useful for administration, troubleshooting, and security investigations.

---

# Related Notes

- [[MAC Address]]
- [[Ethernet]]
- [[Switches]]

