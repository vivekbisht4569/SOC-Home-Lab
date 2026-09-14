# Windows Security Log Investigation using Splunk

##  Objective

The objective of this lab was to learn how to investigate Windows Security logs in Splunk, understand common Windows Event IDs, and build the thought process of a SOC Analyst while performing log analysis.

Unlike previous labs where we focused on Sysmon events, this lab explored native Windows Security Events and how they can be used to investigate authentication activities.

---

# 🛠 Environment

- Windows 11
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows Security Logs
- Sysmon Installed

---

# Skills Practiced

- Searching Windows Security Logs
- Identifying available log sources
- Understanding Windows Event IDs
- Investigating successful logins
- Understanding Logon Types
- Using SPL commands for analysis
- Creating statistical summaries
- Thinking like a SOC Analyst

---

# Step 1 - Verify Available Log Sources

### SPL Query

```spl
index=*
| stats count by source
```

### Purpose

Before starting any investigation, identify which log sources are available inside Splunk.

### Result

Available Sources:

- WinEventLog:Security
- XmlWinEventLog:Microsoft-Windows-Sysmon/Operational

---

# Step 2 - Explore Windows Security Logs

### SPL Query

```spl
source="WinEventLog:Security"
| table _time EventCode Account_Name Logon_Type ComputerName
```

### Purpose

View Windows Security events and identify important Event IDs.

### Observation

Large number of Security Events were available including:

- 4624
- 4672
- 4688
- 5156
- 4798
- 4799

---

# Step 3 - Investigate Successful Logins

### SPL Query

```spl
source="WinEventLog:Security" EventCode=4624
| table _time Account_Name Logon_Type Workstation_Name Source_Network_Address
```

### Purpose

Investigate successful authentication events.

### Observation

Most authentication events were:

- SYSTEM
- Computer Account (LAPTOP-xxxxx$)
- Logon Type 5 (Service Logon)

No suspicious interactive user logins were observed.

---

# Step 4 - Check Failed Logins

### SPL Query

```spl
source="WinEventLog:Security" EventCode=4625
| table _time Account_Name Failure_Reason Source_Network_Address
```

### Purpose

Identify failed authentication attempts.

### Observation

No failed login attempts were found during the selected time range.

---

# Step 5 - Identify Most Active Accounts

### SPL Query

```spl
source="WinEventLog:Security" EventCode=4624
| stats count by Account_Name
```

### Purpose

Determine which accounts generated authentication events.

### Result

Observed Accounts:

- SYSTEM
- LAPTOP-OVFRID7N$
- DefaultAppPool

---

# Step 6 - Analyze Logon Types

### SPL Query

```spl
source="WinEventLog:Security" EventCode=4624
| stats count by Logon_Type
```

### Purpose

Determine how users authenticated.

### Result

Logon Type:

```
5 → Service Logon
```

Meaning Windows Services authenticated automatically rather than interactive users.

---

# Step 7 - View Authentication Timeline

### SPL Query

```spl
source="WinEventLog:Security" EventCode=4624
| timechart span=1m count
```

### Purpose

Visualize authentication activity over time.

---

# Step 8 - Discover Common Windows Event IDs

### SPL Query

```spl
source="WinEventLog:Security"
| stats count by EventCode
| sort EventCode
```

### Purpose

Identify frequently occurring Windows Security Event IDs.

### Important Event IDs Observed

| Event ID | Description |
|-----------|-------------|
|4624|Successful Login|
|4648|Explicit Credentials Used|
|4672|Special Privileges Assigned|
|4688|Process Creation|
|4798|User Group Enumeration|
|4799|Security Group Enumeration|
|5058|Cryptographic Operation|
|5061|Cryptographic Key Access|
|5154|Listening Port|
|5156|Windows Firewall Allowed Connection|

---

# Key SPL Commands Used

```spl
stats
table
sort
timechart
search
```

---

# Key Concepts Learned

- Windows Security Logs contain authentication-related events.
- Event IDs represent different security activities.
- Always identify available log sources before starting an investigation.
- Event ID 4624 represents successful authentication.
- Event ID 4625 represents failed authentication.
- Logon Type provides context about how authentication occurred.
- `stats` is useful for summarizing data.
- `table` displays important fields.
- `timechart` visualizes event frequency.
- Investigations should begin with questions rather than random SPL queries.

---

# SOC Analyst Investigation Workflow

```text
Alert
   │
   ▼
Identify Log Source
   │
   ▼
Find Relevant Event ID
   │
   ▼
Inspect Important Fields
   │
   ▼
Summarize Using Stats
   │
   ▼
Visualize Activity
   │
   ▼
Correlate Events
```

---

# What I Learned

This lab introduced Windows Security Log analysis using Splunk. I learned how to identify available log sources, investigate authentication events, interpret Windows Event IDs, analyze Logon Types, summarize data using SPL commands, and develop a structured investigation methodology similar to that used by SOC Analysts during real-world incident investigations.

---


