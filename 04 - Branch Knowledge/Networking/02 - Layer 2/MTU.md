---
tags:
  - networking
  - osi
  - layer2
  - ethernet
  - mtu
type: concept
status: complete
---

# MTU (Maximum Transmission Unit)

## Definition

The Maximum Transmission Unit (MTU) is the largest amount of payload data that can be transmitted in a single frame without fragmentation.

For standard Ethernet, the default MTU is **1500 bytes**.

---

# Purpose

MTU determines the maximum size of data that can be carried efficiently across a network.

Choosing an appropriate MTU improves performance and reduces fragmentation.

---

# Standard Ethernet MTU

| Network | MTU |
|---------|-----|
| Ethernet | 1500 Bytes |
| Jumbo Frames | 9000 Bytes (Typical) |

---

# Ethernet Frame and MTU

    Ethernet Frame
    +-----------------------------------------------+
    | Header |      Payload (MTU)      |     FCS     |
    +-----------------------------------------------+
               Maximum = 1500 Bytes

The MTU refers only to the payload carried inside the Ethernet frame.

---

# Why MTU Exists

Without an MTU limit:

- Frames could become extremely large.
- Large frames occupy the network longer.
- Errors would require retransmitting larger amounts of data.
- Network devices would require more memory.

MTU provides a balance between efficiency and reliability.

---

# What Happens When Data Exceeds the MTU?

Example:

Application Data = 4000 Bytes

MTU = 1500 Bytes

Result

    4000 Bytes
         │
         ▼
    Fragment into
      ├── 1500
      ├── 1500
      └── 1000

Depending on the protocol and network configuration, fragmentation may occur or the sender may reduce the packet size.

---

# Jumbo Frames

Jumbo Frames allow payloads larger than the standard Ethernet MTU.

Typical MTU

    9000 Bytes

Advantages

- Less protocol overhead
- Better throughput
- Lower CPU utilization

Requirements

Every device on the communication path must support Jumbo Frames.

---

# MTU Mismatch

If devices use different MTU values:

- Fragmentation may occur.
- Packets may be dropped.
- Performance may decrease.
- Some applications may fail.

---

# How to View MTU

## Linux

    ip link show

or

    ip addr

---

## Windows

    netsh interface ipv4 show subinterfaces

---

# Security Perspective

Large or unusual MTU values may indicate:

- Network misconfiguration
- Tunneling protocols
- VPN traffic
- Encapsulation technologies

Attackers may also exploit fragmentation to evade security devices.

---

# Key Takeaways

- MTU is the maximum payload size of a frame.
- Standard Ethernet MTU is 1500 bytes.
- Data larger than the MTU may require fragmentation.
- Jumbo Frames increase efficiency on supported networks.
- Incorrect MTU settings can cause connectivity and performance issues.

---

# Related Notes

- [[Ethernet]]
- [[Ethernet Frame]]
- [[Encapsulation]]
- [[Layer 3]]

