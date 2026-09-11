# 🔐  Windows Password Authentication Investigation (Basic Failed Login Analysis)

## 📌 Overview

In this lab, I investigated **Windows failed authentication events** using **Splunk SIEM**. Unlike a brute-force attack, this lab focused on **basic failed password attempts** generated manually by entering an incorrect password multiple times before logging in successfully.

The objective was to learn the **SOC investigation methodology**, understand how authentication events are recorded in Windows Security Logs, and build an investigation workflow instead of memorizing SPL queries.

---

# 🎯 Objectives

- Generate Windows failed login events manually.
- Investigate Windows Security Event ID **4625**.
- Identify the target account.
- Determine the affected system.
- Find the source of the authentication attempt.
- Build an investigation timeline.
- Verify whether a successful login occurred after failed attempts.
- Learn a repeatable SOC investigation methodology.

---

# 🛠️ Lab Environment

| Component | Purpose |
|-----------|----------|
| Windows 11 | Target Machine |
| Splunk Enterprise | SIEM |
| Universal Forwarder | Log Collection |
| Windows Security Logs | Authentication Logs |

---

# 📚 Windows Event IDs Used

| Event ID | Description |
|----------|-------------|
| **4625** | Failed Logon |
| **4624** | Successful Logon |

---

# 🧪 Attack Simulation

Instead of using Hydra or any brute-force tool, authentication failures were generated manually by:

1. Locking the Windows system.
2. Entering an incorrect password multiple times.
3. Finally logging in with the correct password.

This generated:

- Multiple Event ID **4625**
- One Event ID **4624**

This creates a safe environment to understand authentication investigations before moving to real attack simulations.

---

# 🧠 SOC Investigation Methodology

This investigation follows the same workflow used by SOC analysts.

```text
Alert

   ↓

Understand the Alert

   ↓

Identify Relevant Event IDs

   ↓

Collect Evidence

   ↓

Build Timeline

   ↓

Identify Target User

   ↓

Identify Target Machine

   ↓

Identify Source

   ↓

Check Authentication Outcome

   ↓

Draw Conclusion

   ↓

Write Investigation Report
```

The focus of this methodology is to **answer investigation questions**, not to memorize SPL commands.

---

# 🔍 Investigation Questions

Every investigation starts by asking questions.

### Question 1

How many failed login attempts occurred?

---

### Question 2

Which account was targeted?

---

### Question 3

Which computer was affected?

---

### Question 4

What was the source of the login attempt?

---

### Question 5

What Logon Type was used?

---

### Question 6

When did the attempts begin and end?

---

### Question 7

Was there a successful login after the failed attempts?

---

# 🔎 Investigation Process

## Step 1 — Identify Failed Login Events

Search for Windows failed authentication events.

```spl
source="WinEventLog:Security" EventCode=4625
```

---

## Step 2 — Count Failed Attempts

Determine the total number of failed logins.

```spl
source="WinEventLog:Security" EventCode=4625
| stats count
```

**Observation**

- 9 Failed Login Attempts

---

## Step 3 — Identify Target Account

Determine which account received the failed logins.

```spl
source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name
```

**Observation**

- Target User Identified

---

## Step 4 — Identify Target Computer

Determine the computer receiving authentication attempts.

```spl
source="WinEventLog:Security" EventCode=4625
| stats count by Computer_Name
```

**Observation**

- Target Machine Identified

---

## Step 5 — Build Investigation Timeline

View failed logins chronologically.

```spl
source="WinEventLog:Security" EventCode=4625
| table _time Account_Name Computer_Name
```

**Observation**

- Timeline Created
- Authentication Attempts Recorded Sequentially

---

## Step 6 — Identify Source

Attempt to identify the source IP address.

```spl
source="WinEventLog:Security" EventCode=4625
| table Source_Network_Address
```

If unavailable, inspect the raw event using **Show All Fields** or **Show All Lines**.

---

## Step 7 — Check Logon Type

Identify how authentication was attempted.

```spl
source="WinEventLog:Security" EventCode=4625
| stats count by Logon_Type
```

---

## Step 8 — Check Successful Authentication

Search for successful logins.

```spl
source="WinEventLog:Security" EventCode=4624
```

Compare timestamps to determine whether authentication eventually succeeded.

---

# 📊 Investigation Findings

| Finding | Result |
|----------|--------|
| Failed Login Event | Event ID 4625 |
| Successful Login Event | Event ID 4624 |
| Failed Attempts | 9 |
| Timeline | Created |
| Target User | Identified |
| Target Computer | Identified |
| Source Investigation | Performed |
| Authentication Outcome | Verified |

---

# 📷 Screenshots Included

- Failed Login Events (4625)
- Successful Login Event (4624)
- Timeline
- Account Investigation
- Computer Investigation
- Event Details
- Authentication Sequence

---

# 🧠 Key Learning

This lab emphasized **investigation methodology** rather than SPL syntax.

Instead of asking:

> "Which SPL command should I use?"

I learned to ask:

- What happened?
- Who was targeted?
- Which machine was affected?
- Where did the request originate?
- Did authentication eventually succeed?
- What evidence supports my conclusion?

Only after answering these questions should the incident report be written.

---

# 🛡️ SOC Analyst Workflow Learned

```text
Alert

↓

Understand the Problem

↓

Identify Relevant Logs

↓

Ask Investigation Questions

↓

Collect Evidence

↓

Build Timeline

↓

Correlate Events

↓

Determine Impact

↓

Write Conclusion
```

---

# 📖 Skills Practiced

- Windows Security Log Analysis
- Authentication Investigation
- Event ID 4625 Analysis
- Event ID 4624 Analysis
- Timeline Building
- Evidence Collection
- Investigation Methodology
- Splunk SIEM
- Basic Incident Analysis
- SOC Thinking Process

---

# ✅ Conclusion

This lab focused on understanding how Windows authentication events are investigated within a SIEM environment. By manually generating failed password attempts, I learned to analyze Event IDs **4625** and **4624**, build an investigation timeline, identify affected users and systems, and determine whether authentication eventually succeeded.

The primary outcome of this exercise was developing a structured **SOC investigation mindset**, where every investigation begins with questions, follows evidence, and ends with a documented conclusion rather than relying on memorized SPL queries.