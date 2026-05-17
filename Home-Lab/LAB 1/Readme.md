# SOC-Home-Lab

A practical cybersecurity home lab focused on SOC analysis, attack simulation, Windows log analysis, and detection engineering using Kali Linux, Windows 11, and VirtualBox in an isolated sandbox environment.

---

##  Lab Setup

* Host OS: Windows 11
* Attacker Machine: Kali Linux VM
* Virtualization: VirtualBox
* Network Type: Host-Only Adapter
* Environment: Isolated Sandbox Lab

---

##  Objective

The goal of this lab was to:

* Build a beginner SOC home lab
* Simulate attack traffic safely
* Analyze Windows security logs
* Understand network reconnaissance and detection workflows
* Learn how SOC analysts investigate suspicious activity

---

##  Tools Used

* Kali Linux
* Windows Event Viewer
* Nmap
* Windows Auditing
* VirtualBox
* Networking Fundamentals

---

##  Attack Simulation Performed

### Nmap TCP Scan

```bash
nmap -Pn -sT -p 80 192.168.56.1
```

### Activity Observed

* Inbound traffic from Kali VM
* Source IP detection
* Port 80 HTTP connection attempts
* Windows Filtering Platform logs

---

##  Windows Event Logs Analyzed

### Event IDs

| Event ID | Description                |
| -------- | -------------------------- |
| 5156     | Allowed network connection |
| 5157     | Blocked network connection |

---

##  Key Findings

### Kali Attack Traffic

* Source IP: `192.168.56.101`
* Destination IP: `192.168.56.1`
* Destination Port: `80`
* Direction: `Inbound`

### Normal Windows Traffic

* Outbound HTTPS traffic on port `443`
* Browser/system-generated network activity
* IPv4 and IPv6 traffic observations

---

##  Concepts Learned

* Difference between inbound vs outbound traffic
* Port 80 (HTTP) vs Port 443 (HTTPS)
* How Windows logs network activity
* Importance of auditing configuration
* Log filtering and correlation
* Identifying attacker IP addresses
* Understanding normal vs suspicious traffic

---

##  Challenges Faced

* Empty logs before enabling auditing
* Confusion between IPv4 and IPv6 traffic
* Identifying attacker-generated events among normal system logs

---

##  Solutions Applied

* Enabled Windows auditing using `auditpol`
* Used port-specific Nmap scans for clearer visibility
* Correlated timestamps, ports, and source IP addresses



## ⚠️ Disclaimer

This lab was created strictly for educational purposes inside an isolated sandbox environment. No unauthorized systems were targeted.

---
