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

The Data Link Layer (Layer 2) is the second layer of the OSI model responsible for node-to-node (hop-to-hop) communication over a local network. It organizes raw bits into frames, identifies devices using MAC addresses, detects transmission errors, and controls access to the communication medium.

## Primary Goal

Provide reliable communication between directly connected devices on the same local network (LAN).

## Scope

### Includes
- Framing
- MAC Addressing
- Error Detection (FCS)
- Media Access Control
- Local Delivery
- Ethernet
- Switching
- VLANs

### Excludes
- Routing
- IP Addressing
- End-to-End Reliability
- Application Communication

## Responsibilities

### Framing
Converts raw bits into structured frames.

### Physical Addressing
Uses MAC addresses to identify sender and receiver.

### Error Detection
Detects corrupted frames using Frame Check Sequence (FCS).

### Media Access Control
Controls access to the shared communication medium.

### Local Delivery
Transfers frames between devices on the same LAN.

### Flow Control
Provides link-level flow control where supported.

## Protocol Data Unit (PDU)

| Layer | PDU |
|-------|-----|
| Layer 4 | Segment |
| Layer 3 | Packet |
| Layer 2 | Frame |
| Layer 1 | Bits |

## Address Used

| Layer | Address |
|-------|---------|
| Layer 3 | IP Address |
| Layer 2 | MAC Address |

## Common Devices

- Switch
- Bridge
- Wireless Access Point

## Related Notes

- Ethernet
- Ethernet Frame
- MAC Address
- ARP
- Switches
- VLANs
- Spanning Tree Protocol
- Layer 2 Security

