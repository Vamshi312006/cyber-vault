---
tags:
  - networking
  - osi
  - layer2
  - ethernet
  - frames
type: concept
status: complete
---

# Frame Types

## Definition

A frame type describes how an Ethernet frame is delivered on a Local Area Network (LAN). The destination MAC address determines whether a frame is sent to one device, all devices, or a group of devices.

---

# Types of Frames

1. Unicast
2. Broadcast
3. Multicast

---

# 1. Unicast Frame

## Definition

A unicast frame is sent from one device to exactly one destination device.

## Destination MAC

The MAC address of a single device.

Example

    PC A
      │
      ▼
    Switch
      │
      ▼
    PC B

Only PC B processes the frame.

## Uses

- Web browsing
- SSH
- File transfer
- Email
- Most network communication

---

# 2. Broadcast Frame

## Definition

A broadcast frame is sent to every device within the same broadcast domain.

## Destination MAC

    FF:FF:FF:FF:FF:FF

Every device receives and processes the frame.

Example

    PC A
      │
      ▼
    Switch
   /  |  \
 PC1 PC2 PC3

All connected devices receive the frame.

## Common Uses

- ARP Requests
- DHCP Discover
- Network discovery protocols

---

# 3. Multicast Frame

## Definition

A multicast frame is sent to a specific group of devices instead of every device.

Only devices that have joined the multicast group process the frame.

## Example Destination MAC

    01:00:5E:xx:xx:xx

## Common Uses

- Video streaming
- Live broadcasts
- IPTV
- Routing protocols
- Service discovery

---

# Comparison

| Feature | Unicast | Broadcast | Multicast |
|---------|----------|-----------|-----------|
| Destination | One Device | All Devices | Group of Devices |
| Destination MAC | Specific MAC | FF:FF:FF:FF:FF:FF | Multicast MAC |
| Efficiency | High | Low | Medium |
| Typical Uses | Normal Communication | ARP, DHCP | Streaming, Routing |

---

# Switch Behavior

## Unicast

Forwarded only to the port where the destination device is connected.

## Broadcast

Flooded to every port except the incoming port.

## Multicast

Forwarded only to ports that have interested multicast receivers (when multicast management such as IGMP Snooping is used). Otherwise, many switches flood multicast traffic similarly to broadcasts.

---

# Security Perspective

Broadcast Traffic

- Excessive broadcasts can create broadcast storms.
- Attackers may exploit broadcast traffic for network discovery.

Multicast Traffic

- Can be abused for denial-of-service attacks.
- Improper multicast configuration may leak traffic.

Unicast Traffic

- Generally the most secure and efficient communication method.
- Can still be intercepted through attacks such as ARP spoofing or MAC flooding.

---

# Key Takeaways

- Ethernet supports three frame delivery types.
- Unicast is one-to-one communication.
- Broadcast is one-to-all communication.
- Multicast is one-to-many communication.
- Broadcast traffic should be minimized for better network performance.

---

# Related Notes

- [[Ethernet]]
- [[Ethernet Frame]]
- [[MAC Address]]
- [[Switches]]
- [[ARP]]

