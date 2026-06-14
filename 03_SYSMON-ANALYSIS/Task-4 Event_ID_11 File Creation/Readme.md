#  Sysmon Event ID 11 (File Creation Monitoring)

## Overview

In this lab, I investigated Sysmon Event ID 11 (File Creation) to understand how Windows records file creation activity and how SOC analysts can identify which process created a file.

The objective was to generate file creation activity and analyze the resulting Sysmon logs.

---

## Objective

Learn how to:

- Detect file creation events using Sysmon.
- Identify the process responsible for creating a file.
- Determine which user performed the action.
- Investigate file paths and timestamps.
- Understand how file creation monitoring is used during security investigations.

---

## Lab Environment

### Tools Used

- Windows 11
- Sysmon
- Event Viewer
- Notepad

---

## Activity Performed

I created a text file using Notepad.

### File Created

```text
text.txt
```

### File Location

```text
C:\Users\vivek\Downloads\text.txt
```

Contents of the file:

```text
hello Sysmon vohh
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
Event ID = 11
```

---

## Event Analysis

### Event ID

```text
11
```

Meaning:

```text
File Created
```

---

### Image

```text
Notepad.exe
```

Full Path:

```text
C:\Program Files\WindowsApps\Microsoft.WindowsNotepad...
```

Analysis:

```text
Notepad was the process that created the file.
```

---

### TargetFilename

```text
C:\Users\vivek\Downloads\text.txt
```

Analysis:

```text
This is the exact file created on the system.
```

---

### User

```text
LAPTOP-0VFRID7N\vivek
```

Analysis:

```text
The file was created by the user account "vivek".
```

---

### Process ID

```text
14208
```

Analysis:

```text
Unique process instance responsible for the action.
```

---

### Process GUID

```text
{9ecdab3c-5512-6a2d-5903-000000005001}
```

Analysis:

```text
Unique Sysmon identifier used for event correlation.
```

---

### Creation Time

```text
2026-06-13 13:03:59
```

Analysis:

```text
Timestamp showing when the file was created.
```

---

## SOC Investigation Perspective

Using Event ID 11, an analyst can answer:

```text
Which process created the file?
Who created it?
Where was it created?
When was it created?
```

In this lab:

```text
Notepad.exe
      ↓
Created
      ↓
text.txt
      ↓
Downloads Folder
      ↓
User: vivek
```

---

## Real-World Security Relevance

Event ID 11 is extremely useful for detecting:

- Malware downloads
- Payload drops
- Ransomware activity
- Unauthorized file creation
- Suspicious PowerShell-generated files

Example:

```text
powershell.exe
      ↓
Creates
      ↓
invoice.exe
```

This would immediately attract analyst attention.

---

## Key Learning

Event ID 11 provides visibility into:

```text
Process
    ↓
Creates File
    ↓
Target Filename
    ↓
User
    ↓
Timestamp
```

This allows SOC analysts to track file activity across endpoints.

---

## Skills Learned

1 Sysmon Event ID 11 Analysis

2 File Creation Monitoring

3 Process Attribution

4 User Attribution

5 Event Correlation

6 Endpoint Investigation

7 SOC Investigation Workflow

---

## Conclusion

Successfully generated and investigated a Sysmon Event ID 11 log by creating a file with Notepad.

Verified that Sysmon records:

- The process responsible
- The created file
- The user account
- The creation timestamp

This event provides valuable visibility for threat hunting, malware investigations, and incident response activities.