# Sysmon Process Creation Investigation (Event ID 1)

## Objective

Learn how SOC analysts investigate process execution using Sysmon Event ID 1 and understand the importance of process telemetry, parent-child relationships, command-line analysis, integrity levels, and process hashing.

---

## Lab Environment

| Component | Purpose |
|------------|------------|
| Windows 11 | Endpoint |
| Sysmon | Endpoint Telemetry |
| Event Viewer | Log Investigation |

---

## Event Investigated

### Sysmon Event ID

```text
Event ID: 1
```

### Meaning

```text
Process Creation
```

Sysmon generates Event ID 1 whenever a new process starts on the system.

---

## Event Analysis

### Child Process

```text
Image:
C:\Windows\System32\mmc.exe
```

Meaning:

```text
Microsoft Management Console (MMC) was executed.
```

---

### Command Line

```text
"C:\WINDOWS\system32\mmc.exe"
"C:\WINDOWS\system32\eventvwr.msc"
```

Meaning:

```text
MMC was launched to open Event Viewer.
```

---

### User

```text
LAPTOP-0VFRID7N\vivek
```

Meaning:

```text
The process was executed by the user "vivek".
```

SOC Question:

```text
Who executed the process?
```

Answer:

```text
vivek
```

---

## Parent-Child Process Relationship

### Parent Process

```text
ParentImage:
C:\Windows\system32\eventvwr.exe
```

### Child Process

```text
Image:
C:\Windows\System32\mmc.exe
```

Relationship:

```text
eventvwr.exe
      ↓
    mmc.exe
```

---

### Understanding Parent & Child Processes

A parent process is the process that launches another process.

A child process is the process that gets launched.

Example:

```text
explorer.exe
      ↓
    cmd.exe
```

Parent:

```text
explorer.exe
```

Child:

```text
cmd.exe
```

In this investigation:

```text
eventvwr.exe
      ↓
    mmc.exe
```

Parent:

```text
eventvwr.exe
```

Child:

```text
mmc.exe
```

---

## Integrity Level

```text
IntegrityLevel: High
```

Meaning:

```text
The process was running with elevated administrative privileges.
```

Common Integrity Levels:

| Level | Meaning |
|---------|---------|
| Low | Restricted |
| Medium | Standard User |
| High | Administrator |
| System | SYSTEM Account |

---

## Process ID

```text
ProcessId: 19120
```

Meaning:

```text
Temporary identifier assigned to the running process.
```

---

## Process GUID

```text
ProcessGuid:
{9ecdab3c-f8bd-6a22-0004-000000003501}
```

Meaning:

```text
Unique identifier used to track a process across multiple Sysmon events.
```

Unlike Process IDs, Process GUIDs remain unique and are preferred for investigations.

---

## SHA256 Hash

```text
SHA256:
28CD84B90B09FBBABDE0234197F8963D7A92F4067BC6E3D82CF86A8847040F7
```

Purpose:

- File Identification
- Malware Analysis
- Threat Intelligence
- VirusTotal Searches

SOC analysts often use file hashes to determine whether a process is legitimate or malicious.

---

## SOC Investigation Questions

When reviewing Event ID 1, analysts ask:

1. What process started?
2. Who started it?
3. What launched it?
4. What command was executed?
5. Was it running with elevated privileges?
6. Is the behavior normal?

---

## Assessment

Observed Activity:

```text
eventvwr.exe
      ↓
    mmc.exe
```

Purpose:

```text
User opened Event Viewer, which launched Microsoft Management Console (MMC).
```

Analysis:

```text
Legitimate administrative activity.
```

Verdict:

```text
Benign Process Execution
```

---

## Skills Learned

✅ Sysmon Event ID 1

✅ Process Creation Monitoring

✅ Parent-Child Process Analysis

✅ Command-Line Investigation

✅ User Attribution

✅ Integrity Level Analysis

✅ SHA256 Hash Analysis

✅ Process GUID Tracking

✅ Endpoint Telemetry Investigation

---

## SOC Relevance

Process creation events are among the most valuable sources of telemetry for defenders.

Many attacks can be detected by identifying unusual parent-child process relationships such as:

```text
winword.exe
      ↓
powershell.exe
```

or

```text
excel.exe
      ↓
cmd.exe
```

Understanding these relationships helps analysts detect malware execution, phishing attacks, and post-exploitation activity.

---

## Outcome

Successfully investigated a Sysmon Event ID 1 process creation event, identified the parent and child processes, analyzed command-line execution, reviewed integrity levels, examined file hashes, and applied a real SOC investigation workflow.