# Sysmon Fundamentals for SOC Analysts

## Overview

This phase focused on learning how Sysmon provides detailed endpoint telemetry that helps Security Operations Center (SOC) analysts investigate suspicious activity on Windows systems.

The objective was to understand how to monitor and investigate:

- Process Creation
- Network Connections
- File Creation
- Registry Modifications
- Event Correlation

using Sysmon and Event Viewer.

By the end of this phase, I learned how SOC analysts reconstruct attack timelines using endpoint logs instead of investigating isolated events.

---

# Lab Environment

## Tools Used

- Windows 11
- Sysmon
- Event Viewer
- PowerShell

---

# What is Sysmon?

Sysmon (System Monitor) is a Microsoft Sysinternals tool that extends Windows logging capabilities by recording detailed endpoint activity.

Unlike standard Windows logs, Sysmon provides visibility into:

- Process execution
- Network connections
- File creation
- Registry changes
- Hash values
- Parent-child process relationships

These logs are widely used in SOC environments, threat hunting, malware analysis, and incident response.

---

# Event ID 1 – Process Creation

## Purpose

Records every new process that starts on the system.

### Example

```text
explorer.exe
      ↓
powershell.exe
```

### Key Fields

- Image
- CommandLine
- User
- ParentImage
- ProcessId
- ProcessGuid
- Hashes

### What I Learned

- How processes are created.
- How parent-child relationships work.
- How to identify the process responsible for activity.
- Why command-line visibility is critical during investigations.

### SOC Use Cases

- PowerShell abuse
- Malware execution
- LOLBins
- Ransomware execution
- Macro-based attacks

---

# Event ID 3 – Network Connection

## Purpose

Records outbound network connections created by processes.

### Example

```text
powershell.exe
      ↓
google.com
      ↓
Port 443
```

### Key Fields

- Image
- DestinationIP
- DestinationHostname
- DestinationPort
- Protocol

### What I Learned

- How Sysmon links processes to network activity.
- How to identify remote hosts.
- How to investigate outbound communications.
- How to determine whether traffic is expected or suspicious.

### SOC Use Cases

- Command & Control (C2)
- Malware beaconing
- Data exfiltration
- Reverse shells

---

# Event ID 11 – File Creation

## Purpose

Records file creation activity.

### Example

```text
powershell.exe
      ↓
Creates File
```

### Key Fields

- Image
- TargetFilename
- User

### What I Learned

- How to identify which process created a file.
- How malware drops files onto systems.
- Why temporary directories are commonly abused.
- How Sysmon captures file creation artifacts.

### SOC Use Cases

- Malware droppers
- Payload staging
- Tool deployment
- Persistence files

---

# Event ID 13 – Registry Modification

## Purpose

Records registry value modifications.

### Example

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### Key Fields

- Image
- TargetObject
- Details
- User

### What I Learned

- How Windows persistence mechanisms work.
- How attackers abuse Run Keys.
- How Sysmon captures registry modifications.
- How to investigate persistence techniques.

### SOC Use Cases

- Persistence
- Startup execution
- Malware configuration
- Defense evasion

---

# Parent-Child Process Relationships

One of the most important concepts learned during this phase was understanding process lineage.

### Example

```text
explorer.exe
      ↓
powershell.exe
```

This relationship shows that Explorer launched PowerShell.

### Why It Matters

SOC analysts use parent-child relationships to determine:

- Who launched a process
- Whether execution is expected
- Whether a process chain is suspicious

### Suspicious Examples

```text
winword.exe
      ↓
powershell.exe
```

```text
excel.exe
      ↓
cmd.exe
```

```text
outlook.exe
      ↓
powershell.exe
```

These relationships are commonly observed during malicious activity.

---

# Event Correlation Lab

After learning individual Event IDs, I performed event correlation to build a complete activity timeline.

### Activities Performed

1. Launch PowerShell
2. Generate network activity
3. Create files
4. Modify the registry

### Timeline

```text
explorer.exe
      ↓
powershell.exe started
      ↓
Connected to Google over HTTPS
      ↓
Created temporary files
      ↓
Modified Registry Run Key
      ↓
Configured notepad.exe persistence
```

### What I Learned

SOC analysts rarely investigate a single log.

Instead, they build a story by correlating:

```text
Process
      ↓
Network
      ↓
File
      ↓
Registry
```

This provides complete visibility into system activity.

---

# SOC Investigation Methodology

Throughout the labs, I practiced the investigation workflow used by SOC analysts.

### Process Investigation

```text
What started?

Who started it?

What is the parent process?
```

### Network Investigation

```text
Where did it connect?

Which port was used?

Is the destination trusted?
```

### File Investigation

```text
What file was created?

Where was it created?

Which process created it?
```

### Registry Investigation

```text
Which key changed?

What value was written?

Is persistence being established?
```

---

# Key Skills Acquired

1 Sysmon Deployment and Usage

2 Event Viewer Analysis

3 Process Investigation

4 Parent-Child Process Analysis

5 Network Connection Monitoring

6 File Creation Monitoring

7 Registry Monitoring

8 Event Correlation

9 Timeline Reconstruction

10 Basic Threat Hunting

11 SOC Investigation Methodology

---

# Quick Event Reference

| Event ID | Description |
|-----------|-------------|
| 1 | Process Creation |
| 3 | Network Connection |
| 11 | File Creation |
| 13 | Registry Modification |

---

# Analyst Mindset

A beginner investigates individual logs:

```text
Event ID 1

Event ID 3

Event ID 11

Event ID 13
```

A SOC analyst investigates behavior:

```text
Process
      ↓
Network
      ↓
File
      ↓
Registry
      ↓
Timeline
      ↓
Verdict
```

---

# Conclusion

Successfully completed the Sysmon Fundamentals phase and gained hands-on experience investigating endpoint activity using Sysmon logs.

This phase provided a strong foundation in:

- Windows telemetry
- Endpoint monitoring
- Event correlation
- SOC investigation workflows

The skills learned here directly support future work with SIEM platforms such as Splunk, where these same events are collected, searched, correlated, and investigated at scale.

---

