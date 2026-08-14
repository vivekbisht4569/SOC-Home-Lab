# 🔐 SSH Traffic Analysis using Wireshark

## 📌 Lab Overview

This lab focused on capturing and analyzing an **SSH (Secure Shell)** session using Wireshark.

The objective was to understand how SSH communication appears at the packet level, identify the SSH handshake and key-exchange process, and understand what information remains visible even when the session is encrypted.

---

## 🎯 Objectives

- Understand how SSH communication works.
- Identify SSH traffic in Wireshark.
- Analyze SSHv2 protocol negotiation.
- Identify the TCP connection and port 22.
- Observe the SSH key-exchange process.
- Identify when encryption begins.
- Understand what a SOC analyst can and cannot see from encrypted SSH traffic.

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Client | Windows |
| Client IP | `192.168.56.1` |
| Server | Kali Linux |
| Server IP | `192.168.56.102` |
| Protocol | SSHv2 |
| Transport | TCP |
| Port | `22` |
| Tool | Wireshark |

### Network Flow

```text
Windows
192.168.56.1
     |
     | TCP / 22
     ↓
Kali Linux
192.168.56.102