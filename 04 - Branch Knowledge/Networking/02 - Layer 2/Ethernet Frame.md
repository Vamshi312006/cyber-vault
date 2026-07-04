---
tags:
  - networking
  - osi
  - layer2
  - ethernet
  - frame
type: concept
status: complete
---

# Ethernet Frame

## Definition

An Ethernet Frame is the Protocol Data Unit (PDU) of the Data Link Layer. It encapsulates Layer 3 packets with Layer 2 information so they can be transmitted across an Ethernet network.

---

# Purpose

The Ethernet frame provides:

- Source identification
- Destination identification
- Packet encapsulation
- Error detection
- Frame boundaries

---

# Ethernet Frame Structure

    +-----------+----------------+-------------+-----------+---------+------+
    | Preamble  | Destination MAC| Source MAC  | EtherType | Payload | FCS  |
    +-----------+----------------+-------------+-----------+---------+------+

---

# Fields

## 1. Preamble

Size: 7 Bytes

Purpose:

- Synchronizes sender and receiver.
- Indicates that a frame is about to begin.

---

## 2. Start Frame Delimiter (SFD)

Size: 1 Byte

Purpose:

- Marks the actual beginning of the Ethernet frame.

---

## 3. Destination MAC Address

Size: 6 Bytes

Purpose:

Specifies the intended recipient of the frame.

Example:

    00:1A:2B:3C:4D:5E

---

## 4. Source MAC Address

Size: 6 Bytes

Purpose:

Identifies the device that transmitted the frame.

---

## 5. EtherType

Size: 2 Bytes

Purpose:

Identifies which Layer 3 protocol is encapsulated.

Examples:

| EtherType | Protocol |
|-----------|----------|
| 0x0800 | IPv4 |
| 0x0806 | ARP |
| 0x86DD | IPv6 |

---

## 6. Payload

Size:

46–1500 Bytes

Purpose:

Contains the Layer 3 packet.

Examples:

- IPv4 Packet
- IPv6 Packet
- ARP Packet

---

## 7. Padding

Purpose:

If the payload is smaller than 46 bytes, padding is added to meet the minimum Ethernet frame size.

---

## 8. Frame Check Sequence (FCS)

Size: 4 Bytes

Purpose:

Detect transmission errors using Cyclic Redundancy Check (CRC).

If the calculated CRC does not match the received CRC, the frame is discarded.

---

# Frame Size

| Field | Size |
|--------|------|
| Minimum Frame | 64 Bytes |
| Maximum Frame | 1518 Bytes |
| Maximum Payload | 1500 Bytes |

---

# Encapsulation Process

    Layer 3 Packet
           │
           ▼
    Ethernet Header Added
           │
           ▼
    Payload Added
           │
           ▼
    FCS Calculated
           │
           ▼
    Ethernet Frame

---

# Decapsulation Process

    Ethernet Frame
           │
           ▼
    Verify FCS
           │
           ▼
    Remove Ethernet Header
           │
           ▼
    Deliver Layer 3 Packet

---

# Key Takeaways

- The Ethernet Frame is the PDU of Layer 2.
- It encapsulates Layer 3 packets.
- MAC addresses identify sender and receiver.
- EtherType identifies the encapsulated protocol.
- FCS detects transmission errors.
- Frames are forwarded by switches using MAC addresses.

---

# Related Notes

- [[Ethernet]]
- [[MAC Address]]
- [[Frame Types]]
- [[MTU]]
- [[Encapsulation]]

