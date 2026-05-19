# 🌐 Network Reconnaissance & Wireshark Traffic Analysis

## 📌 Overview

This lab demonstrates how to build a small SOC-style network monitoring environment to simulate reconnaissance activity and analyze network traffic using Wireshark. The objective was to understand how SOC analysts monitor traffic, detect reconnaissance scans, analyze packets, investigate suspicious activity, and troubleshoot network visibility issues.

---

# 🏗️ Lab Architecture

```text
               ┌──────────────────┐
               │  Kali Linux VM   │
               │   (Attacker)     │
               └────────┬─────────┘
                        │
                Nmap / ICMP / TCP
                        │
                        ▼
               ┌──────────────────┐
               │  Windows Host    │
               │  (Target System) │
               └────────┬─────────┘
                        │
                        ▼
               ┌──────────────────┐
               │    Wireshark     │
               │  Packet Capture  │
               └──────────────────┘
```

---

# ⚙️ Environment Setup

| Component        | Details           |
| ---------------- | ----------------- |
| Host OS          | Windows           |
| Attacker Machine | Kali Linux VM     |
| Virtualization   | VirtualBox        |
| Network Mode     | Host-Only Adapter |
| Packet Analyzer  | Wireshark         |
| Recon Tool       | Nmap              |

---

# 🌐 Network Configuration

## 🔹 VirtualBox Adapter Mode

**Host-Only Adapter**

### Why Host-Only?

* Creates an isolated cybersecurity lab
* Enables safe attack simulation
* Allows VM ↔ Host communication
* Prevents accidental internet exposure

---

# 🔍 IP Address Identification

## 🐉 Kali Linux IP

```bash
ip a
```

Example:

```text
192.168.56.102
```

## 🖥️ Windows Host IP

```cmd
ipconfig
```

Example:

```text
192.168.56.1
```

---

# 🦈 Wireshark Packet Capture

## ⚠️ Initial Problem

Initially, no packets appeared in Wireshark because the incorrect network interface was selected.

### ❌ Incorrect Interface

```text
Wi-Fi
```

### ✅ Correct Interface

```text
VirtualBox Host-Only Ethernet Adapter
```

## 📘 Key SOC Lesson

> Effective monitoring depends entirely on visibility.

This reflects real-world SOC environments where analysts must identify the correct network segment before beginning investigations.

---

# 📡 ICMP Traffic Analysis

## 🔹 Ping Test

From Kali Linux:

```bash
ping 192.168.56.1
```

## 🔹 Wireshark Filter

```text
icmp
```

## 🔍 Observed Traffic

| Packet Type  | Description                       |
| ------------ | --------------------------------- |
| Echo Request | Kali checking target availability |
| Echo Reply   | Windows responding to ping        |

---

# 🌐 TCP Traffic Analysis

## 🔹 Wireshark Filter

```text
tcp
```

# 🤝 TCP 3-Way Handshake

```text
SYN → SYN-ACK → ACK
```

| Step    | Meaning                |
| ------- | ---------------------- |
| SYN     | Connection request     |
| SYN-ACK | Server acknowledgment  |
| ACK     | Connection established |

---

# 🔎 Reconnaissance Using Nmap

## 🔹 Basic Scan

```bash
nmap 192.168.56.1
```

# 🚨 SYN Scan Detection

## 🔹 Wireshark Filter

```text
tcp.flags.syn == 1
```

## 🔍 Detection Findings

Nmap reconnaissance activity generated:

* Multiple SYN packets
* Rapid port probing
* Reconnaissance behavior patterns

---

# 🌐 Important Ports Identified

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTPS   |
| 445  | SMB     |
| 3389 | RDP     |

---

# 🎯 Filtering Attacker Traffic

## 🔹 Wireshark Filter

```text
ip.src == 192.168.56.102
```

This filter isolates traffic originating from the Kali Linux attacker machine.

---

# 🔄 Follow TCP Stream

## 🔹 Feature Used

```text
Follow → TCP Stream
```

## 🔹 SOC Use Cases

* Malware traffic analysis
* HTTP inspection
* Credential investigation
* Command-and-control (C2) analysis

---

# 💾 Packet Capture Evidence

## 📁 Saved PCAP File

```text
lab2_recon.pcapng
```

---

# 🛡️ Skills Developed

## 🔹 Technical Skills

* Packet capture
* Wireshark filtering
* TCP/IP analysis
* Reconnaissance detection
* Nmap analysis
* Network troubleshooting

## 🔹 Security Analyst Skills

* Traffic monitoring
* Reconnaissance detection
* Visibility troubleshooting
* Investigation mindset

---

# 🚀 Future Applications

This lab directly supports learning for:

* SOC Analyst roles
* Blue Team operations
* Threat Hunting
* SIEM analysis
* Incident Response
* Splunk/Wazuh integration
* Network Security Monitoring

---

# 📚 Key Takeaway

> “Effective cybersecurity monitoring starts with visibility.”

This lab demonstrated how attackers generate reconnaissance traffic and how defenders can detect and analyze that activity using packet analysis tools like Wireshark.

---

# 📌 Tools Used

* Kali Linux
* Wireshark
* Nmap
* VirtualBox
* Windows Networking Tools

---

# 📖 Author Notes

This project was created as part of hands-on SOC Analyst and Blue Team training to develop practical network monitoring and traffic analysis skills in a controlled lab environment.
