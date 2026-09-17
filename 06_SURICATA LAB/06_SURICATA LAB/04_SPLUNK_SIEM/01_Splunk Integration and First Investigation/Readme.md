# Splunk Integration & First Investigations

## Objective

The goal of this lab was to integrate Sysmon logs into Splunk and begin performing basic SOC analyst investigations using SPL (Search Processing Language).

---

## Lab Architecture

Windows 11 Host
↓
Sysmon
↓
Windows Event Logs
↓
Splunk Universal Forwarder
↓
Splunk Enterprise
↓
Search & Investigation

---

## Tools Used

- Windows 11
- Sysmon
- Splunk Enterprise
- Splunk Universal Forwarder
- Event Viewer

---

## Step 1 - Verify Sysmon Data Collection

Verified Sysmon was generating:

- Event ID 1 (Process Creation)
- Event ID 3 (Network Connection)
- Event ID 11 (File Creation)
- Event ID 13 (Registry Modification)

Events were confirmed in:

Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational

---

## Step 2 - Configure Splunk

Installed:

- Splunk Enterprise
- Splunk Universal Forwarder

Configured:

- Receiving Port: 9997
- Local Windows Host as Forwarder

Verified:

SplunkForwarder: Running

---

## Step 3 - Verify Data Ingestion

Initial search:

```spl
index=*
```

Result:

```text
37,000+ events indexed
```

Confirmed Sysmon source:

```text
XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

---

## First Splunk Investigations

### View All Sysmon Logs

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
```

---

### Process Creation Events

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=1
```

Purpose:

- View process executions
- Identify parent-child relationships

---

### Network Connection Events

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=3
```

Purpose:

- View outbound network connections
- Identify destination IPs and ports

---

### File Creation Events

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=11
```

Purpose:

- Monitor file creation activity

---

### Registry Modification Events

```spl
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=13
```

Purpose:

- Detect persistence mechanisms
- Monitor registry changes

---

## SPL Commands Learned

### table

Display selected fields.

```spl
| table _time Image User CommandLine
```

---

### stats

Aggregate data.

```spl
| stats count by Image
```

Purpose:

Count process executions.

---

### sort

Sort results.

```spl
| sort -count
```

Purpose:

Show highest-frequency results first.

---

### top

Display most common values.

```spl
| top Image
```

Purpose:

Identify frequently executed processes.

---

### rare

Display least common values.

```spl
| rare Image
```

Purpose:

Identify unusual process executions.

---

## SOC Analyst Mindset

Instead of asking:

"What happened?"

A SOC analyst asks:

- Which process executed?
- Who executed it?
- What was the parent process?
- Did it make a network connection?
- Did it create files?
- Did it modify the registry?

---

## Process Investigation Workflow

Event ID 1
↓
Process Created

Event ID 3
↓
Network Activity

Event ID 11
↓
File Activity

Event ID 13
↓
Registry Activity

This workflow forms the basis of endpoint investigations.

---

## Skills Learned

1 Splunk Installation

2 Universal Forwarder Configuration

3 Log Ingestion Verification

4 Sysmon Log Collection

5 SPL Basics

6 Process Investigation

7 Network Investigation

8 File Activity Monitoring

9 Registry Monitoring

10 SOC Investigation Workflow

---

## Conclusion

Successfully integrated Sysmon telemetry into Splunk and performed initial investigations using SPL.

Built a complete SOC-style log pipeline capable of collecting and analyzing endpoint activity for threat hunting and incident response.