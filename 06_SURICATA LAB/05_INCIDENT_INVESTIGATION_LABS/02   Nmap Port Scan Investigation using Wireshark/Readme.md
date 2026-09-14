#  Nmap Port Scan Investigation using Wireshark

## Objective

The goal of this lab is to understand how a **TCP SYN (Nmap) Port Scan** looks on the network using Wireshark.

Instead of just learning Wireshark filters, the focus of this lab is to learn **how a SOC Analyst investigates a reconnaissance attack**.

---

# Lab Setup

| Machine | Role |
|----------|------|
| Windows Host | Victim |
| Kali Linux VM | Attacker |
| Wireshark | Network Monitoring Tool |

---

# Attack Performed

From Kali Linux, a basic Nmap scan was performed against the Windows machine.

```bash
nmap <Windows-IP>
```

Example

```bash
nmap 192.168.56.1
```

The scan generated network traffic which was captured using Wireshark running on the Windows machine.

---

# Investigation Methodology

Always investigate in the same order.

```
Attack Happens
        ↓
Capture Network Traffic
        ↓
Identify Attacker
        ↓
Identify Victim
        ↓
Identify Protocol
        ↓
Identify Attack Type
        ↓
Find Targeted Ports
        ↓
Identify Open Ports
        ↓
Write Investigation Report
```

This methodology should be followed for every future Wireshark investigation.

---

# Investigation Steps

---

## Step 1 - Start Packet Capture

Open Wireshark.

Select the active network adapter.

Start capturing packets.

---

## Step 2 - Launch the Attack

From Kali Linux run:

```bash
nmap <Windows-IP>
```

Wait for the scan to complete.

Stop packet capture.

---

## Step 3 - Identify the Attacker

Observe the **Source IP Address**.

Result:

```
Attacker IP
192.168.56.101
```

---

## Step 4 - Identify the Victim

Observe the **Destination IP Address**.

Result:

```
Victim IP
192.168.56.1
```

---

## Step 5 - Identify the Protocol

Observe the Protocol column.

Result

```
TCP
```

This confirms the scan is using TCP packets.

---

## Step 6 - Detect SYN Scan

Observe the **Info** column.

Packets contained

```
[SYN]
```

Only SYN packets were being sent to many destination ports.

This indicates reconnaissance activity.

---

## Step 7 - Detect Open Ports

Apply the following Wireshark filter.

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

This filter shows only ports that replied with **SYN-ACK**.

A SYN-ACK response means the destination port is **Open**.

---

# Open Ports Found

The Windows machine responded on the following ports.

| Port | Service |
|------|---------|
| 80 | HTTP |
| 139 | NetBIOS Session Service |
| 445 | SMB |

These ports were successfully discovered by the attacker.

---

# Why SYN-ACK is Important

TCP communication begins with a three-way handshake.

```
Client
   │
   ├── SYN ─────────────►
   │
Server
   │
   ◄────── SYN-ACK ─────
   │
Client
   │
   ├──── ACK ──────────►
```

If a server replies with **SYN-ACK**, the port is open.

If the server replies with **RST**, the port is closed.

Nmap uses this behavior to discover open ports without fully establishing the connection.

---

# Investigation Findings

## Attacker

```
192.168.56.101
```

## Victim

```
192.168.56.1
```

## Attack Type

```
TCP SYN Port Scan
```

## Tool Used

```
Nmap
```

## Protocol

```
TCP
```

## Open Ports

```
80
139
445
```

---

# What an Attacker Learns

After the scan, the attacker now knows that the target machine is running the following services.

```
Port 80
↓

HTTP Service

Port 139
↓

NetBIOS Service

Port 445
↓

SMB Service
```

This information helps attackers decide their next attack.

For example,

```
Port 445
↓

SMB Enumeration

↓

Credential Attacks

↓

Lateral Movement
```

Reconnaissance is the first phase of almost every cyber attack.

---

# SOC Investigation Summary

```
Alert Generated

↓

Identify Source IP

↓

Identify Destination IP

↓

Confirm Protocol

↓

Identify Scan Type

↓

Identify Open Ports

↓

Determine Possible Target Services

↓

Write Incident Report
```

---

# Skills Learned

- Capturing network traffic using Wireshark
- Understanding TCP SYN scans
- Identifying attacker and victim
- Detecting reconnaissance activity
- Using Wireshark display filters
- Identifying open ports using SYN-ACK packets
- Understanding the TCP three-way handshake
- Following a structured SOC investigation methodology

---

# Wireshark Filters Used

### Show TCP SYN packets

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

---

### Show Open Ports (SYN-ACK)

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

---

# Conclusion

In this lab, a TCP SYN scan was launched from a Kali Linux virtual machine against a Windows host.

Using Wireshark, the scan traffic was captured and investigated.

The investigation identified:

- The attacker IP address
- The victim IP address
- The attack type
- The TCP protocol used
- The open ports exposed on the Windows system

This lab demonstrates how SOC analysts detect and investigate reconnaissance attacks before attackers attempt exploitation.