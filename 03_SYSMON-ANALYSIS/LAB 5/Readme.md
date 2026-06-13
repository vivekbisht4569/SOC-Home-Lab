# Sysmon Event ID 13 (Registry Modification Monitoring)

## Overview

In this lab, I investigated **Sysmon Event ID 13 (Registry Value Set)** to understand how Windows records registry modifications and how SOC analysts detect persistence mechanisms used by attackers.

The objective was to generate a registry modification event and analyze the resulting Sysmon log.

---

## Objective

Learn how to:

- Detect registry modifications using Sysmon.
- Identify the process responsible for modifying the registry.
- Determine which user performed the action.
- Analyze registry paths and values.
- Understand how attackers use registry keys for persistence.

---

## Lab Environment

### Tools Used

- Windows 11
- Sysmon
- Event Viewer
- PowerShell

---

## Activity Performed

A registry value was created using PowerShell.

### Registry Path

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### Registry Value Name

```text
SOC-LAB
```

### Registry Value Data

```text
notepad.exe
```

---

## Investigation Steps

### Open Sysmon Logs

```text
Event Viewer
→ Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```

### Filter Logs

```text
Event ID = 13
```

---

## Event Analysis

### Event ID

```text
13
```

**Meaning:**

```text
Registry Value Set / Modified
```

---

### Process

```text
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

**Analysis:**

PowerShell was responsible for modifying the registry.

---

### User

```text
LAPTOP-0VFRID7N\vivek
```

**Analysis:**

The registry modification was performed under the user account "vivek".

---

### Registry Key Modified

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run\SOC-LAB
```

**Analysis:**

The modification occurred inside the **Run Key**, a common persistence location in Windows.

---

### Value Written

```text
notepad.exe
```

**Analysis:**

Windows will attempt to launch Notepad automatically when the user logs in.

---

### Process ID

```text
6536
```

**Analysis:**

The specific PowerShell process responsible for the registry modification.

---

### Process GUID

```text
{9ecdab3c-59b8-6a2d-5809-000000005001}
```

**Analysis:**

A unique Sysmon identifier used for event correlation across logs.

---

## Investigation Methodology

During analysis, the following questions were answered:

### 1. What Happened?

```text
A registry value was created.
```

### 2. Who Performed It?

```text
User: vivek
```

### 3. Which Process Did It?

```text
powershell.exe
```

### 4. Which Registry Key Was Modified?

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### 5. What Value Was Added?

```text
notepad.exe
```

### 6. What Is the Impact?

```text
Notepad will launch automatically when the user logs in.
```

---

## Why SOC Analysts Care

Registry Run Keys are commonly abused by attackers to maintain persistence.

Example:

```text
powershell.exe
      ↓
Creates Registry Run Key
      ↓
Adds payload.exe
      ↓
System Reboots
      ↓
Payload Executes Automatically
```

This allows malware to survive reboots and maintain access.

---

## SOC Investigation Perspective

From the observed event:

```text
PowerShell
      ↓
Modified Registry
      ↓
Run Key
      ↓
Added Value: notepad.exe
```

Since the activity was intentionally generated during lab testing and the value points to a legitimate application, the activity is considered:

```text
Benign
```

No malicious indicators were observed.

---

## Skills Learned

1 Sysmon Event ID 13 Analysis

2 Registry Monitoring

3 Persistence Detection

4 Registry Run Key Investigation

5 Process Attribution

6 User Attribution

7 SOC Investigation Workflow

8 Threat Hunting Fundamentals

---

## Key Learning

Event ID 13 allows analysts to correlate:

```text
Process
    ↓
Registry Modification
    ↓
Registry Key
    ↓
Value Written
    ↓
User
```

This visibility is critical for detecting persistence techniques used by attackers.

---

## Conclusion

Successfully generated and investigated a Sysmon Event ID 13 log.

Verified that Sysmon records:

- The process responsible for the modification
- The affected registry key
- The value written
- The user account responsible

Event ID 13 is one of the most valuable telemetry sources for detecting persistence mechanisms and investigating suspicious registry activity.