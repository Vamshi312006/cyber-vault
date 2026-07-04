---
tags:
  - networking
  - osi
  - layer2
  - ethernet
type: concept
status: complete
---

# Ethernet

## Definition

Ethernet is the most widely used Layer 2 (Data Link Layer) technology for Local Area Networks (LANs). It defines how devices communicate over wired networks by specifying frame formats, MAC addressing, media access methods, and physical transmission standards.

---

# Why Ethernet Exists

Computers connected to the same network require a common set of rules to communicate.

Ethernet standardizes:

- Frame format
- Device addressing
- Error detection
- Medium access
- Physical transmission

Without Ethernet, devices from different manufacturers would not be able to communicate reliably.

---

# OSI Layers

Ethernet operates across two layers.

| Layer | Responsibility |
|--------|----------------|
| Layer 2 | Frames, MAC Addresses, Error Detection |
| Layer 1 | Electrical, Optical, or Radio Signal Transmission |

---

# Main Components

- Ethernet Frame
- MAC Address
- Network Interface Card (NIC)
- Switch
- Transmission Medium (Copper/Fiber)

---

# Ethernet Standards

| Standard | Speed | Medium |
|----------|-------|--------|
| 10BASE-T | 10 Mbps | Twisted Pair |
| 100BASE-TX | 100 Mbps | Twisted Pair |
| 1000BASE-T | 1 Gbps | Twisted Pair |
| 10GBASE-T | 10 Gbps | Twisted Pair |

---

# Ethernet Communication

A typical communication follows this sequence.

    Computer A
         │
         ▼
    Ethernet Frame
         │
         ▼
       Switch
         │
         ▼
    Computer B

---

# Ethernet Characteristics

- Uses MAC Addresses for local communication.
- Uses Frames as the Protocol Data Unit (PDU).
- Performs error detection using Frame Check Sequence (FCS).
- Operates only within a local network.
- Requires Layer 3 devices (routers) to communicate with different networks.

---

# Advantages

- High performance
- Reliable
- Vendor independent
- Scalable
- Easy to deploy
- Low cost

---

# Limitations

- Limited to local network communication.
- Does not perform routing.
- Traditional Ethernet does not provide encryption.
- Error detection only; no error correction.

---

# Real-World Example

When a browser sends a request to another computer on the same LAN:

1. The application generates data.
2. Layer 4 creates a segment.
3. Layer 3 creates an IP packet.
4. Ethernet encapsulates the packet into an Ethernet frame.
5. The switch forwards the frame using MAC addresses.
6. The destination computer receives the frame.

---

# Key Takeaways

- Ethernet is the most common Layer 2 technology.
- It defines how wired LAN communication works.
- Ethernet uses frames and MAC addresses.
- Switches forward Ethernet frames.
- Routers connect different Ethernet networks.

---

# Related Notes

- [[Ethernet Frame]]
- [[MAC Address]]
- [[Switches]]
- [[ARP]]
- [[Responsibilities]]

