#  Sysmon Event Correlation Lab

## Overview

In this lab, I learned how SOC analysts correlate multiple Sysmon events to build a complete attack timeline instead of investigating individual logs separately.

The objective was to generate multiple Sysmon events using PowerShell and then reconstruct the sequence of actions from the collected telemetry.

---

## Objective

Learn how to correlate:

- Event ID 1 (Process Creation)
- Event ID 3 (Network Connection)
- Event ID 11 (File Creation)
- Event ID 13 (Registry Modification)

and build a complete incident timeline.

---

## Tools Used

- Windows 11
- Sysmon
- Event Viewer
- PowerShell

---

## Lab Scenario

A PowerShell process was launched and used to:

1. Establish a network connection.
2. Create files.
3. Modify the Windows Registry.

The goal was to investigate all generated events and correlate them into a single timeline.

---

# Event 1 – Process Creation

### Event ID

```text
1
```

### Process

```text
powershell.exe
```

### Parent Process

```text
explorer.exe
```

### Process ID

```text
9196
```

### Analysis

PowerShell was launched by Explorer.

Parent-child relationship:

```text
explorer.exe
        ↓
powershell.exe
```

This indicates that the user manually started PowerShell from the desktop or Start Menu.

---

# Event 2 – Network Connection

### Event ID

```text
3
```

### Process

```text
powershell.exe
```

### Protocol

```text
TCP
```

### Destination Port

```text
443
```

### Destination Host

```text
pnbomb-aa-in-x0e.1e100.net
```

### Analysis

PowerShell established an outbound HTTPS connection.

Port:

```text
443
```

represents:

```text
HTTPS
```

The destination hostname belongs to Google's infrastructure.

SOC Observation:

```text
PowerShell initiated an outbound encrypted connection.
```

---

# Event 3 – File Creation

### Event ID

```text
11
```

### Process

```text
powershell.exe
```

### File Created

```text
C:\Users\vivek\AppData\Local\Temp\
PSScriptPolicyTest_0pq25us3.h3e.ps1
```

### Analysis

PowerShell automatically generated temporary script files while executing commands.

This demonstrates how Sysmon records files created by a process.

SOC Observation:

```text
PowerShell created temporary artifacts within the user profile.
```

---

# Event 4 – Registry Modification

### Event ID

```text
13
```

### Process

```text
powershell.exe
```

### Registry Path

```text
HKU\...\Software\Microsoft\Windows\CurrentVersion\Run\Test
```

### Value

```text
notepad.exe
```

### Analysis

PowerShell modified a Windows Run Key.

Run Keys are commonly used for persistence because programs stored here automatically execute after user logon.

SOC Observation:

```text
A persistence mechanism was created using a Run Key.
```

---

# Event Correlation

Instead of analyzing events individually, the investigation combined them into a timeline.

### Timeline

```text
User launched PowerShell
        ↓
PowerShell started
(Event ID 1)

        ↓
PowerShell connected to Google over HTTPS
(Event ID 3)

        ↓
PowerShell created temporary files
(Event ID 11)

        ↓
PowerShell modified a Run Key
(Event ID 13)
```

---

# SOC Investigation Methodology

For each event, the following questions were answered:

### Process Creation

```text
What process started?
Who launched it?
What is the parent process?
```

### Network Connection

```text
Where did it connect?
Which port was used?
Was the connection initiated locally?
```

### File Creation

```text
What file was created?
Where was it created?
Which process created it?
```

### Registry Modification

```text
Which key was modified?
What value was written?
Could this be persistence?
```

---

# Why Event Correlation Matters

A single event rarely tells the full story.

SOC analysts combine multiple telemetry sources to understand attacker behavior.

Example:

```text
Process
      ↓
Network Activity
      ↓
File Creation
      ↓
Persistence
```

This sequence is commonly observed during malware execution.

---

# Skills Learned

1 Sysmon Event Correlation

2 Timeline Reconstruction

3 Process Investigation

4 Network Investigation

5 File Activity Analysis

6 Registry Analysis

7 Parent-Child Process Relationships

8 SOC Investigation Methodology

9 Basic Threat Hunting

---

# Key Takeaways

Event correlation allows analysts to move beyond individual logs and reconstruct an entire attack chain.

By linking:

```text
Event ID 1
Event ID 3
Event ID 11
Event ID 13
```

it becomes possible to understand:

```text
What happened
Who did it
Which process was involved
What files were created
What registry changes occurred
Whether persistence was established
```

---

# Conclusion

Successfully generated and investigated multiple Sysmon events and correlated them into a complete timeline.

This lab demonstrated how SOC analysts use process, network, file, and registry telemetry together to investigate suspicious activity and build incident timelines.