#  SOC Investigation Methodology

## 📌 Overview

This document explains the investigation methodology followed throughout my SOC Home Lab.

The goal of a SOC analyst is not to memorize SIEM queries, but to understand how to investigate security events by asking the right questions, identifying evidence, analyzing logs, and reaching a conclusion.

A SIEM tool like Splunk is only used to answer investigation questions.

---

# 🧠 Core SOC Mindset

A SOC analyst does not start with:

```
Which SPL query should I run?
```

A SOC analyst starts with:

```
What happened?

What evidence do I need?

Where can I find that evidence?
```

---

# 🔍 SOC Investigation Workflow

Every investigation follows this process:

```text
Alert / Suspicious Activity

        ↓

1. Understand the Alert

        ↓

2. Identify Activity Category

        ↓

3. Identify Log Source

        ↓

4. Find Relevant Event ID

        ↓

5. Identify Important Fields

        ↓

6. Analyze Evidence

        ↓

7. Correlate Related Activity

        ↓

8. Determine Verdict
```

---

# Step 1 — Understand The Alert

## Question

What is happening?

Before opening Splunk, identify the activity type.

Examples:

| Activity | Category |
|---------|----------|
| Failed Login | Authentication |
| PowerShell Execution | Process |
| Unknown EXE Started | Process |
| External Connection | Network |
| File Created | File Activity |
| Registry Modified | Persistence |

---

# Step 2 — Identify Log Source

## Question

Where will the evidence exist?

Examples:

| Evidence Needed | Log Source |
|----------------|------------|
| Login Activity | Windows Security Logs |
| Process Execution | Windows Security / Sysmon |
| Network Connections | Sysmon / Firewall |
| Registry Changes | Sysmon |
| File Creation | Sysmon |

---

# Step 3 — Identify Event ID

## Question

Which event records this behavior?

Examples:

## Windows Security

| Event ID | Activity |
|----------|----------|
|4624|Successful Login|
|4625|Failed Login|
|4672|Privilege Assigned|
|4688|Process Creation|
|4720|User Created|
|4732|User Added To Group|

---

## Sysmon

| Event ID | Activity |
|----------|----------|
|1|Process Creation|
|3|Network Connection|
|7|DLL Loaded|
|11|File Created|
|13|Registry Modified|

---

# Step 4 — Identify Important Fields

Never analyze every field.

Focus on evidence.

---

## Universal Questions

Every investigation answers:

```
WHO?

WHAT?

WHEN?

WHERE?

HOW?
```

---

## Important Fields

### Time

```
_time
```

Question:

When did it happen?

---

### User

```
Account_Name
User
```

Question:

Who performed the action?

---

### Host

```
ComputerName
host
```

Question:

Where did it happen?

---

### Process

```
Image
New_Process_Name
```

Question:

What executed?

---

### Parent Process

```
ParentImage
Creator_Process_Name
```

Question:

How was it launched?

---

### Command

```
CommandLine
Process_Command_Line
```

Question:

What exactly was executed?

---

# Step 5 — Build Investigation View

After identifying important fields, create a clean investigation view.

Example thinking:

```
I need:

Time
+
User
+
Process
+
Parent Process
+
Command
```

The goal is to remove noise and keep useful evidence.

---

# Step 6 — Analyze Behavior

Ask:

## Is the activity expected?

Example:

Normal:

```
explorer.exe

      ↓

notepad.exe
```

User opened Notepad.

---

Suspicious:

```
WINWORD.EXE

      ↓

powershell.exe

      ↓

unknown.exe
```

Office spawning PowerShell is unusual.

---

# Step 7 — Correlate Events

Never investigate one event alone.

Build a timeline.

Example:

```
Successful Login

        ↓

Process Execution

        ↓

Network Connection

        ↓

File Created

        ↓

Registry Modified
```

---

# Investigation Examples

## Authentication Alert

Thinking:

```
What happened?
↓
Login Activity

Where?
↓
Windows Security

Evidence?
↓
4624 / 4625

Need:
↓
User
IP
Logon Type
Time
```

---

## Process Alert

Thinking:

```
What happened?
↓
Process Started

Where?
↓
Security Logs / Sysmon

Evidence?
↓
4688 / Sysmon ID 1

Need:
↓
Process
User
Parent Process
Command Line
```

---

# SPL Thinking Methodology

Do not memorize complete queries.

Understand the purpose.

---

## Find Something

Purpose:

```
Show matching events
```

---

## Display Evidence

Purpose:

```
Show important fields
```

Example:

```
table
```

---

## Summarize

Purpose:

```
How many?
Who most?
Which process most?
```

Example:

```
stats count by
```

---

## Timeline

Purpose:

```
When did activity happen?
```

Example:

```
timechart
```

---

# SOC Investigation Formula

Remember:

```text
Question

    ↓

Evidence Needed

    ↓

Log Source

    ↓

Event ID

    ↓

Important Fields

    ↓

SPL

    ↓

Analysis

    ↓

Conclusion
```

---

# Analyst Verdict Template

Every investigation ends with:

## Alert

What triggered?

## Evidence Found

What logs show?

## Timeline

What happened first?

## Impact

Was anything affected?

## Verdict

Choose:

```
Benign

or

Suspicious
```

## Reason

Explain using evidence.

---

# Key Lesson

Logs are evidence.

SIEM is a tool.

SPL is a language.

The real SOC skill is:

```
Knowing what questions to ask
and knowing where evidence exists.
```

---

# Author

**Vivek Singh Bisht**

Cybersecurity Student  
SOC Analyst Journey  
Hands-on Blue Team Home Lab

```
Think Like An Analyst.
Investigate Like A Detective.
Respond Like A Defender.
```