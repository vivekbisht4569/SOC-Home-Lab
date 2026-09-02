# Lab 02: Understanding Suricata EVE JSON

## 🎯 Objective

The objective of this lab is to understand how Suricata records
network activity in `eve.json` and how the raw JSON can be viewed
and processed using PowerShell.

The focus of this part of the lab is understanding the telemetry
rather than creating new detection rules.

---

## 🏗️ Lab Environment

### Kali Linux

IP Address:
192.168.56.103

### Windows

IP Address:
192.168.56.1

### Suricata

C:\Suricata\

### EVE JSON Log

C:\Suricata\log\eve.json

---

# 1. Start Suricata

Suricata was started using the previously created startup batch file:

C:\Suricata\start-suricata.bat

After starting, Suricata displayed:

Engine started

This confirmed that the Suricata engine was running and monitoring
the configured network interface.

### Evidence

![](images/02_Start_suricata_engine.png)

---

# 2. Generate Controlled Traffic from Kali

To give Suricata network traffic to observe, controlled ICMP traffic
was generated from Kali Linux toward the Windows host.

### Kali IP

192.168.56.103

### Windows IP

192.168.56.1

### Command

    ping 192.168.56.1

The ping was allowed to run for a few seconds and then stopped using:

    Ctrl + C

### Traffic Flow

    Kali
    192.168.56.103
          |
          | ICMP
          v
    Windows
    192.168.56.1
          |
          v
    Npcap
          |
          v
    Suricata
          |
          v
    EVE JSON

### Evidence

![](images/01_Kali_Ping.png)

---

# 3. View the Raw EVE JSON

Suricata stores structured event information in:

    C:\Suricata\log\eve.json

The raw EVE JSON file was viewed from Windows CMD using:

    type C:\Suricata\log\eve.json

The output contained a large amount of JSON data.

Different event types can appear in the file, for example:

    flow
    alert
    netflow
    stats

The raw output is machine-oriented and can be difficult for a
human to read directly in the terminal.

### Evidence

![Raw EVE JSON](images/03_eve_json_file.png)

---

# 4. Understanding the Problem with Raw JSON

The raw `eve.json` file contains a large amount of structured
information.

For example, an event can contain fields such as:

    timestamp
    event_type
    src_ip
    dest_ip
    proto

Trying to manually read the entire JSON file is inefficient.

Therefore, PowerShell was introduced to process and organize
the telemetry.

The objective was not to memorize a large PowerShell command,
but to understand how the command is constructed.

---

# 5. PowerShell — Get-Content

The first PowerShell command introduced was:

    Get-Content C:\Suricata\log\eve.json

### Purpose

`Get-Content` reads the contents of a file.

In this case:

    Get-Content
         ↓
    Read eve.json

This is similar to using:

    type C:\Suricata\log\eve.json

in Windows CMD.

---

# 6. PowerShell — Pipeline

The PowerShell pipeline symbol is:

    |

The pipeline passes the output of one command to another command.

For example:

    Get-Content C:\Suricata\log\eve.json | Select-String "ICMP"

The process is:

    Get-Content
         ↓
    Read eve.json
         ↓
    Pipeline |
         ↓
    Select-String
         ↓
    Search for ICMP

This is useful during log investigation because output from one
command can be processed by another command.

---

# 7. PowerShell — ConvertFrom-Json

The next concept introduced was:

    ConvertFrom-Json

Suricata's EVE file contains JSON data.

`ConvertFrom-Json` converts JSON text into PowerShell objects.

The command used was:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json }

### Concept

    JSON text
        ↓
    ConvertFrom-Json
        ↓
    PowerShell object
        ↓
    Individual fields can be accessed

This allows the JSON data to be processed instead of treating it
as plain text.

---

# 8. PowerShell — Select-Object

After converting the JSON into objects, only useful fields can be
selected.

The fields selected for the initial investigation were:

    timestamp
    event_type
    src_ip
    dest_ip
    proto

Command:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json } | Select-Object timestamp,event_type,src_ip,dest_ip,proto

### Purpose

`Select-Object` allows specific fields to be extracted from the
larger JSON object.

Instead of looking at every field, we can focus on information
useful for initial network investigation.

---

# 9. PowerShell — Format-Table

The output can be displayed in a more readable table using:

    Format-Table -AutoSize

Complete command:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json } | Select-Object timestamp,event_type,src_ip,dest_ip,proto | Format-Table -AutoSize

The purpose of `Format-Table -AutoSize` is to make the selected
telemetry easier to read.

### Evidence

![PowerShell EVE Investigation](images/04_powershell_command.png)

---

# 10. PowerShell — Where-Object

`Where-Object` was introduced to filter specific events.

Example:

    Where-Object {$_.event_type -eq "flow"}

This means:

    event_type
        ↓
    Is it equal to "flow"?
        ↓
    YES → keep the event
    NO  → discard the event

This allows specific event types to be investigated instead of
looking through every event in the EVE JSON file.

---

# 🧠 What We Have Learned So Far

The investigation pipeline developed so far is:

    eve.json
       ↓
    Get-Content
       ↓
    Read log
       ↓
    ConvertFrom-Json
       ↓
    Understand JSON as objects
       ↓
    Where-Object
       ↓
    Filter events
       ↓
    Select-Object
       ↓
    Extract useful fields
       ↓
    Format-Table
       ↓
    Human-readable output

---

# 🔄 Suricata Telemetry Flow

The basic process understood in this lab is:

    Network Traffic
          ↓
       Npcap
          ↓
      Suricata
          ↓
       EVE JSON
          ↓
    Log Processing
          ↓
    Useful Telemetry
          ↓
    SOC Investigation

---

# 🧠 Important Concepts

- `eve.json` contains structured Suricata telemetry.
- Raw EVE JSON is difficult to read manually.
- PowerShell can be used to process JSON logs.
- `Get-Content` reads the log file.
- `|` passes output between commands.
- `ConvertFrom-Json` converts JSON text into PowerShell objects.
- `Where-Object` filters events.
- `Select-Object` extracts required fields.
- `Format-Table` makes output easier to read.
- EVE JSON is designed to provide structured telemetry that can
  later be consumed by security monitoring and SIEM systems.

---

# 🎯 SOC Perspective

The important methodology from this part of the lab is:

    Traffic
       ↓
    Observation
       ↓
    Telemetry
       ↓
    Filtering
       ↓
    Relevant fields
       ↓
    Investigation

The goal is not simply to read JSON.

The goal is to take a large amount of security telemetry and
extract the information needed to understand what happened.

---

