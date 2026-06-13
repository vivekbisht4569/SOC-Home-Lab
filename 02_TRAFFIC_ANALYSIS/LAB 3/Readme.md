# Wireshark Basics & Packet Analysis

## Objective

Learn the fundamentals of packet analysis using Wireshark and understand how network traffic flows between systems.

---

## Lab Environment

| Machine | Role |
|----------|----------|
| Kali Linux | Traffic Generator |
| Windows 11 | Packet Capture |
| VirtualBox Host-Only Adapter | Network |

---

## Tools Used

- Wireshark
- Kali Linux
- Windows 11
- ICMP (Ping)

---

## Traffic Generated

Command:

```bash
ping 192.168.56.1
```

Purpose:
- Generate ICMP traffic
- Observe packet flow
- Understand source and destination communication

---

## Investigation

Applied Filter:

```text
icmp
```

Observed:

- Echo Request
- Echo Reply

Traffic Flow:

Kali Linux
↓
Echo Request
↓
Windows 11
↓
Echo Reply
↓
Kali Linux

---

## Key Learnings

- Packets are the basic unit of network communication.
- Source IP identifies the sender.
- Destination IP identifies the receiver.
- ICMP is commonly used for connectivity testing.
- Wireshark provides packet-level visibility.

---

## SOC Relevance

SOC analysts use packet captures to:
- Investigate suspicious traffic
- Identify communicating systems
- Analyze protocols
- Validate network connectivity

---

## Outcome

Successfully captured and analyzed ICMP traffic while learning packet analysis fundamentals.