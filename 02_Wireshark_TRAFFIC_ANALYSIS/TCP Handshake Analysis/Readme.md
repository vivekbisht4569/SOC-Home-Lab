# TCP Handshake Analysis

## Objective

Understand how TCP connections are established and learn how to identify the TCP Three-Way Handshake using Wireshark.

---

## Lab Environment

| Machine | Role |
|----------|----------|
| Kali Linux | Scanner |
| Windows 11 | Target |
| VirtualBox Host-Only Adapter | Network |

---

## Tools Used

- Wireshark
- Kali Linux
- Nmap
- Windows 11

---

## Traffic Generated

Command:

```bash
nmap -sT -p 445 192.168.56.1
```

Purpose:
- Generate TCP traffic
- Observe TCP connection establishment
- Analyze handshake packets

---

## Investigation

Applied Filter:

```text
tcp.port == 445
```

Observed TCP Three-Way Handshake:

```text
SYN
↓
SYN-ACK
↓
ACK
```

Example:

192.168.56.101 → 192.168.56.1
[SYN]

192.168.56.1 → 192.168.56.101
[SYN, ACK]

192.168.56.101 → 192.168.56.1
[ACK]

---

## Key Learnings

- TCP uses a Three-Way Handshake.
- SYN initiates communication.
- SYN-ACK acknowledges and accepts the request.
- ACK completes the connection.
- Most modern applications use TCP.

---

## SOC Relevance

SOC analysts investigate:
- SYN floods
- Port scans
- Failed handshakes
- Network reconnaissance
- Service enumeration

Understanding TCP handshakes helps identify attacker behavior and network anomalies.

---

## Outcome

Successfully captured and analyzed TCP connection establishment using Wireshark and learned how TCP communication begins between systems.