# Splunk SIEM Fundamentals

## Overview

This phase introduces Splunk SIEM (Security Information and Event Management), one of the most widely used platforms in Security Operations Centers (SOCs).

Before installing Splunk, the goal was to understand:

- What a SIEM is
- Why organizations use SIEMs
- Splunk architecture
- How logs flow through Splunk
- Core Splunk terminology
- How Sysmon logs will later be investigated using Splunk

This phase builds the foundation required before ingesting Windows, Sysmon, and Kali attack logs.

---

# What is a SIEM?

SIEM stands for:

```text
Security Information and Event Management
```

A SIEM collects logs from multiple systems and provides a centralized platform for searching, monitoring, investigating, and alerting on security events.

---

## Without SIEM

```text
Windows PC
     ↓
Event Viewer

Linux Server
     ↓
Log Files

Firewall
     ↓
Device Logs
```

Analysts must manually access each device.

---

## With SIEM

```text
Windows
Linux
Firewall
Servers
Applications
      ↓
      ↓
      ↓
    Splunk
```

All logs are centralized and searchable from a single location.

---

# Why Organizations Use SIEM

Large environments may contain:

```text
500+ Workstations
100+ Servers
Firewalls
Cloud Services
Security Tools
```

Manually checking logs is impossible.

A SIEM provides:

- Centralized visibility
- Faster investigations
- Threat detection
- Alerting
- Compliance reporting
- Threat hunting capabilities

---

# What Splunk Does

Splunk is a SIEM platform that:

```text
Collects Logs
Stores Logs
Indexes Logs
Searches Logs
Correlates Events
Creates Alerts
Builds Dashboards
```

SOC analysts use Splunk daily to investigate security incidents.

---

# Splunk Architecture

Understanding Splunk architecture is essential for SOC analysts.

---

## Universal Forwarder

### Purpose

Collect logs from endpoints and send them to Splunk.

### Example

```text
Windows Host
      ↓
Universal Forwarder
```

Responsibilities:

- Collect logs
- Forward logs
- Lightweight agent

---

## Indexer

### Purpose

Store and organize incoming logs.

Think of the Indexer as:

```text
Database for Security Logs
```

Responsibilities:

- Receive logs
- Index logs
- Store logs
- Make logs searchable

---

## Search Head

### Purpose

The interface used by analysts.

Responsibilities:

- Run searches
- Investigate incidents
- Build dashboards
- Create reports
- Perform threat hunting

---

# Future Home Lab Architecture

The final home lab will look like:

```text
Kali Linux (Attacker)
          ↓

Windows 11 (Victim)
     + Sysmon
     + Security Logs

          ↓

Universal Forwarder

          ↓

Splunk Enterprise
```

This will allow:

```text
Brute Force Attacks
Nmap Scans
PowerShell Activity
File Creation
Registry Modifications
```

to be investigated directly inside Splunk.

---

# Core Splunk Terminology

---

## Event

An event is a single log record.

Example:

```text
Sysmon Event ID 1

powershell.exe started
```

One log = One event

---

## Index

An index is a collection of logs.

Example:

```spl
index=sysmon
```

Meaning:

```text
Search Sysmon logs.
```

---

## Sourcetype

Identifies the format or type of logs.

Examples:

```text
WinEventLog

XmlWinEventLog

Sysmon
```

Used by Splunk to correctly parse events.

---

## Source

Identifies where the logs came from.

Examples:

```text
Sysmon

Security

PowerShell

Firewall
```

---

# Basic Splunk Searches

---

## Show Everything

```spl
*
```

Meaning:

```text
Display all available events.
```

---

## Show Sysmon Events

```spl
index=sysmon
```

Meaning:

```text
Display all Sysmon logs.
```

---

## Process Creation Events

```spl
index=sysmon EventCode=1
```

Meaning:

```text
Display Sysmon Event ID 1 logs.
```

---

## Network Connection Events

```spl
index=sysmon EventCode=3
```

Meaning:

```text
Display Sysmon Event ID 3 logs.
```

---

## File Creation Events

```spl
index=sysmon EventCode=11
```

Meaning:

```text
Display Sysmon Event ID 11 logs.
```

---

## Registry Modification Events

```spl
index=sysmon EventCode=13
```

Meaning:

```text
Display Sysmon Event ID 13 logs.
```

---

# Key Realization

The investigation process does not change.

Previously:

```text
Event Viewer
      ↓
Filter Event ID
      ↓
Investigate
```

Now:

```text
Splunk
      ↓
Search Event Code
      ↓
Investigate
```

The logs remain the same.

Only the platform changes.

---

# SOC Analyst Workflow

SOC analysts typically investigate:

```text
Process Creation
      ↓
Network Connections
      ↓
File Creation
      ↓
Registry Modifications
      ↓
Build Timeline
      ↓
Determine Impact
```

This workflow directly aligns with the Sysmon labs completed during Phase 3.

---

# Skills Learned

1 Understanding SIEM Concepts

2 Understanding Splunk Architecture

3 Understanding Universal Forwarders

4 Understanding Indexers

5 Understanding Search Heads

6 Understanding Events

7 Understanding Indexes

8 Understanding Sourcetypes

9 Understanding Sources

10 Basic Splunk Search Syntax

---

# Quick Revision Notes

## SIEM

```text
Security Information and Event Management
```

Centralized log collection and analysis platform.

---

## Universal Forwarder

```text
Collects Logs
Sends Logs
```

---

## Indexer

```text
Stores Logs
Indexes Logs
```

---

## Search Head

```text
Searches Logs
Builds Dashboards
Performs Investigations
```

---

## Event

```text
Single Log Entry
```

---

## Index

```text
Collection of Logs
```

---

## Sourcetype

```text
Type of Log
```

---

## Source

```text
Where Log Came From
```

---

# Interview Questions

### What is a SIEM?

```text
A platform used to collect, store, search, analyze, and investigate security logs from multiple systems.
```

---

### What is a Universal Forwarder?

```text
A lightweight Splunk agent used to collect and forward logs.
```

---

### What is an Index?

```text
A storage location where Splunk stores searchable logs.
```

---

### What is an Event?

```text
A single log entry.
```

---

### What is a Search Head?

```text
The component analysts use to search and investigate logs.
```

---

# Conclusion

Successfully completed the theoretical foundation of Splunk SIEM.

Learned how SIEM platforms collect, store, and analyze logs, understood Splunk architecture, and explored the core concepts required before building a fully operational SOC lab.

This knowledge prepares the environment for:

```text
Splunk Installation
↓
Sysmon Log Ingestion
↓
Kali Attack Detection
↓
SOC Investigations
↓
Threat Hunting
```

