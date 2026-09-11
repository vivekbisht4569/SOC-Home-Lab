# 🚨 Incident Investigation Labs

## 📌 Overview

This repository contains hands-on **SOC Incident Investigation Labs** performed in a home lab environment using **Splunk Enterprise**, **Windows Security Logs**, **Sysmon**, **Wireshark**, and other security tools.

Unlike traditional tool-focused labs, these investigations are designed to simulate the daily workflow of a **Security Operations Center (SOC) Analyst**. Each lab begins with an alert, follows a structured investigation methodology, collects evidence, builds timelines, identifies Indicators of Compromise (IOCs), and concludes with an incident report.

The primary objective is to develop **analytical thinking and investigation skills**, rather than simply memorizing SPL queries or Windows Event IDs.

---

# 🎯 Objectives

- Develop a SOC analyst investigation mindset.
- Practice investigating realistic security incidents.
- Learn evidence-based incident analysis.
- Correlate multiple log sources.
- Build investigation timelines.
- Identify Indicators of Compromise (IOCs).
- Improve Splunk investigation skills.
- Document findings in professional incident reports.

---

# 🛠️ Lab Environment

| Component | Purpose |
|-----------|----------|
| Windows 11 | Victim Machine |
| Kali Linux | Attacker Machine |
| Splunk Enterprise | SIEM |
| Universal Forwarder | Log Collection |
| Sysmon | Endpoint Telemetry |
| Windows Security Logs | Authentication & System Logs |
| Wireshark | Network Traffic Analysis |

---

# 🧠 SOC Investigation Methodology

Every investigation follows the same structured workflow.

```text
Alert Received
      │
      ▼
Understand the Alert
      │
      ▼
Identify Relevant Logs
      │
      ▼
Collect Evidence
      │
      ▼
Build Timeline
      │
      ▼
Identify Indicators of Compromise (IOCs)
      │
      ▼
Correlate Events
      │
      ▼
Determine Impact
      │
      ▼
Draw Conclusion
      │
      ▼
Write Incident Report
```

---

# 🔍 Investigation Philosophy

Every investigation begins with **questions**, not queries.

Instead of asking:

> "Which SPL query should I use?"

I first ask:

- What happened?
- When did it happen?
- Who was targeted?
- Which machine was affected?
- Where did the activity originate?
- What evidence supports this activity?
- Did the attack succeed?
- What is the overall impact?

Only after defining these questions do I use SPL to gather evidence.

---

# 📂 Investigation Workflow

Each incident investigation follows this process:

1. Receive Alert
2. Understand the Scenario
3. Identify Relevant Log Sources
4. Collect Evidence
5. Build Timeline
6. Analyze User Activity
7. Analyze Host Activity
8. Identify Network Indicators
9. Correlate Related Events
10. Determine Attack Outcome
11. Document Findings
12. Write Final Incident Report

---

# 📑 Standard Investigation Template

Every lab includes:

- Incident Scenario
- Investigation Objectives
- Environment Setup
- Relevant Event IDs
- SPL Queries
- Investigation Methodology
- Timeline Analysis
- IOC Analysis
- Findings
- Screenshots
- Incident Report
- Lessons Learned

---

# 📚 Planned Incident Investigations

| Lab | Status |
|------|--------|
| Windows Failed Authentication Investigation | ✅ |
| Windows Brute Force Investigation | ⏳ |
| PowerShell Attack Investigation | ⏳ |
| Suspicious Process Investigation | ⏳ |
| RDP Attack Investigation | ⏳ |
| Privilege Escalation Investigation | ⏳ |
| Malware Execution Investigation | ⏳ |
| Persistence Investigation | ⏳ |
| Lateral Movement Investigation | ⏳ |
| Data Exfiltration Investigation | ⏳ |
| Insider Threat Investigation | ⏳ |
| Multi-Stage Attack Investigation | ⏳ |

---

# 🛡️ Skills Demonstrated

- SOC Investigation Methodology
- Incident Response
- Threat Hunting
- Log Analysis
- Windows Security Event Analysis
- Sysmon Analysis
- Splunk SIEM
- Wireshark Analysis
- Timeline Analysis
- IOC Identification
- Evidence Collection
- Security Documentation

---

# 🎯 Goal

The goal of this repository is not only to learn Splunk or Windows Event IDs, but to think and investigate like a SOC Analyst.

Every investigation focuses on understanding **what happened, why it happened, and how to prove it using evidence**, following a structured methodology similar to real-world Security Operations Centers.