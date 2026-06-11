# Sysmon Configuration & Telemetry Validation

## Objective

Understand how Sysmon configurations control telemetry collection and investigate why certain Sysmon Event IDs are not being generated.

---

## Background

After successfully investigating Sysmon Event ID 1 (Process Creation), the next goal was to analyze Sysmon's active configuration and determine why network connection events were not appearing.

---

## Tools Used

- Windows 11
- Sysmon
- Command Prompt (Administrator)
- Event Viewer

---

## Investigation

### Verify Active Sysmon Configuration

Navigated to the Sysmon directory and executed:

```cmd
Sysmon64.exe -c
```

This command displays the currently active Sysmon configuration.

---

## Findings

### Enabled Telemetry

```text
Process Creation: Enabled
```

Result:

```text
Event ID 1
(Process Creation)
```

was successfully generated and visible in:

Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational

---

### Disabled Telemetry

Observed:

```text
Network Connection: Disabled
```

Result:

```text
Event ID 3
(Network Connection)
```

was not generated despite creating network activity using:

- Google Chrome
- PowerShell
- Ping
- Test-NetConnection

---

## Analysis

The investigation confirmed that Sysmon does not automatically collect every type of telemetry after installation.

Sysmon behavior depends on the active configuration file.

Because Network Connection logging was disabled, Sysmon did not generate Event ID 3 entries.

---

## Key Learning

Installing Sysmon does not automatically provide complete visibility.

A SOC analyst must verify:

- What telemetry is enabled
- What telemetry is disabled
- Which events are being collected
- Which visibility gaps exist

---

## SOC Relevance

Missing telemetry creates blind spots during investigations.

For example:

If Event ID 3 is disabled:

```text
Process Execution  → Visible ✅

Network Activity  → Not Visible ❌
```

Analysts can see that a process executed but cannot determine:

- Which IP it connected to
- Which port was used
- Which protocol was involved

This significantly limits incident investigations.

---

## Important SOC Concept

```text
Tool Installed
≠
Telemetry Collected
```

Proper visibility requires:

```text
Tool Installation
+
Configuration
+
Telemetry Collection
```

---

## Outcome

Successfully reviewed the active Sysmon configuration and identified why network connection events were not appearing.

Learned how Sysmon configurations control telemetry collection and how missing telemetry can impact SOC investigations.

---

## Skills Learned

1 Sysmon Configuration Review

2 Telemetry Validation

3 Logging Verification

4 Visibility Gap Identification

5 Event Collection Analysis

6 SOC Investigation Mindset

---
