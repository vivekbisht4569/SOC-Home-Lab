# 🛡️ SOC Home Lab — Phase 1 Task 2  
## Failed Login Detection & Authentication Monitoring

This lab focuses on detecting and investigating failed authentication attempts using Windows Security Logs inside an isolated SOC home lab environment.

---

# 🎯 Objective

Simulate failed SMB login attempts from a Kali Linux attacker machine and investigate Windows authentication telemetry using Event Viewer.

The goal was to understand:
- Failed login detection
- Source IP identification
- Network authentication behavior
- Basic brute-force attack indicators
- Windows Event ID analysis

---

# 🖥️ Lab Environment

| Machine | Role | IP Address |
|---|---|---|
| Windows 11 | Victim Machine | 192.168.56.1 |
| Kali Linux VM | Attacker Machine | 192.168.56.101 |

---

# ⚙️ Technologies Used

- Kali Linux
- Windows 11
- VirtualBox
- NetExec
- SMB Protocol
- Windows Event Viewer
- Windows Security Logs

---

# 🌐 Network Configuration

Used:
- VirtualBox Host-Only Adapter

Reason:
- isolated environment
- proper attacker IP visibility
- realistic internal lab traffic
- cleaner security telemetry

---

# 🔥 Attack Simulation

Generated failed SMB authentication attempts from Kali Linux using:

```bash
netexec smb 192.168.56.1 -u Administrator -p WrongPassword