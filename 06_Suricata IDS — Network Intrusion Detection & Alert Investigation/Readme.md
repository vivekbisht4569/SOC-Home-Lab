# Suricata IDS — Network Detection & Investigation

## 📌 Overview

This lab focuses on learning and practicing **Suricata**, an open-source **Network Intrusion Detection and Prevention System (IDS/IPS)**.

The goal is to understand how network traffic is monitored, how detection rules work, how alerts are generated, and how a SOC analyst investigates those alerts.

This lab will contain **8–10 practical exercises**, progressing from basic Suricata setup to SOC-style network investigation.

## 🧪 Lab Environment

* **Windows Host** — Monitoring / Suricata
* **Kali Linux VM** — Attacker
* **Suricata** — Network IDS
* **Wireshark** — Packet Analysis
* **VirtualBox** — Virtualization

## 🎯 Objectives

* Understand Suricata architecture
* Configure Suricata for network monitoring
* Capture and analyze network traffic
* Understand Suricata rules and signatures
* Generate controlled network activity
* Detect reconnaissance and suspicious traffic
* Investigate Suricata alerts
* Create and test custom detection rules
* Correlate Suricata alerts with Wireshark
* Map detections to MITRE ATT&CK
* Practice SOC-style incident investigation

## 🔬 Planned Labs

1. Suricata Installation & Architecture
2. Packet Capture & Traffic Visibility
3. First Detection Rule & Alert
4. Nmap Reconnaissance Detection
5. ICMP & Network Discovery Detection
6. HTTP Traffic & Web Attack Detection
7. SSH / Brute-Force Detection
8. Custom Detection Engineering
9. Suricata + Wireshark Correlation
10. Mini SOC Incident Investigation

## 🧠 Investigation Methodology

```text
Alert
 ↓
Identify Source
 ↓
Identify Destination
 ↓
Understand Activity
 ↓
Analyze Evidence
 ↓
Correlate with Other Telemetry
 ↓
Determine Severity
 ↓
MITRE ATT&CK Mapping
 ↓
Conclusion
```
