# Detection Rules

A practical introduction to building, applying, tuning, and monitoring security detection rules using logs and SIEM platforms.

This section focuses on how a SOC analyst converts suspicious behaviour into a measurable detection rule that can generate useful and actionable alerts.

---

# 1. What is a Detection Rule?

A detection rule is a logical condition used to identify malicious or suspicious behaviour from security data.

A good detection rule generally:

1. Identifies suspicious or malicious behaviour.
2. Triggers an alert when the defined conditions are met.
3. Works inside a SIEM, IDS/IPS, or custom detection script.

### Basic Detection Flow

```text
Logs
  ↓
Detection Logic
  ↓
Suspicious Activity
  ↓
Alert
  ↓
SOC Analyst Investigation
  ↓
Tune Rule
  ↓
Repeat
```

---

# 2. Detection Rule Development Process

## Step 1 — Identify What You Want to Detect

Start by deciding what security behaviour should generate an alert.

Focus on high-value activities such as:

- Failed login attempts
- Brute-force attacks
- Logins from unusual geography
- Large outbound traffic
- Potential data exfiltration
- Privilege escalation
- Suspicious administrative commands

### Example

```text
Detect Failed Login Attempts
             ↓
        Brute Force
             ↓
           Alert
```

The important point is to start with the behaviour you want to detect rather than immediately writing a complicated rule.

---

# 3. Capture Relevant Logs

A detection rule requires reliable log data.

Common security log sources include:

1. Authentication Logs
2. System Logs
3. Network Logs
4. Application Logs
5. Firewall Logs
6. SIEM-collected Logs

### Example

For detecting brute-force activity, authentication logs may contain:

- Username
- Source IP
- Timestamp
- Login Status
- Authentication Method

For privilege-escalation detection, system or audit logs may contain:

- User
- Command
- Timestamp
- Process
- Privilege Level
- Source

The quality of a detection depends heavily on the quality and availability of the underlying logs.

---

# 4. Define Detection Criteria

Detection criteria describe exactly when a rule should trigger.

Three important components are:

## Threshold

The number of events required before an alert is generated.

### Example

```text
5 failed login attempts
```

## Pattern

The specific keyword, event, command, IP, field, or behaviour being searched for.

### Example

```text
"failed login"
```

## Time Window

The period in which the events must occur.

### Example

```text
5 failed logins
     +
Same Source IP
     +
Within 10 minutes
        ↓
Generate Alert
```

Therefore, a basic detection can be represented as:

```text
Pattern + Threshold + Time Window
```

---

# 5. Example — Failed Login / Brute Force Detection

## Objective

Detect possible brute-force login activity.

### Detection Logic

```text
IF
    failed login attempts >= threshold
AND
    attempts originate from the same source IP
AND
    events occur within a defined time window
THEN
    generate alert
```

### Example

```text
5 Failed Login Attempts
          +
     Same Source IP
          +
     Within 10 Minutes
          ↓
Potential Brute Force Attack
          ↓
         Alert
```

The rule should detect repeated authentication failures rather than a single normal password mistake.

---

# 6. Configuration / Rule Format

A practical detection rule can be thought of using three main elements:

1. Threshold
2. Pattern
3. Time Window

### Example

```text
Threshold:
5 events

Pattern:
Failed login attempt

Time Window:
10 minutes
```

### Combined

```text

5 failed login attempts
from the same IP
within 10 minutes
        ↓
      Alert
```

This makes the detection measurable and repeatable.

---

# 7. Applying Detection Rules in a SIEM

Detection rules can be implemented in different security monitoring platforms.

Examples:

- Splunk
- ELK / Kibana
- Wazuh
- Suricata
- Custom Python detection scripts

The exact syntax changes between platforms, but the detection logic remains similar.

### General Workflow

```text
Logs
 ↓
Search / Filter
 ↓
Detection Conditions
 ↓
Threshold / Correlation
 ↓
Alert
```

---

# 8. Splunk SPL Example

A simple Splunk query for identifying repeated failed login attempts:

```spl
index=auth_logs "failed login attempt"
| stats count by src_ip
| where count >= 5
| table src_ip count
```

### What This Does

```text
Search Authentication Logs
          ↓
Find Failed Login Events
          ↓
Group Events by Source IP
          ↓
Count Failed Attempts
          ↓
Check if Count >= 5
          ↓
Show Suspicious IP
```

This can later be extended with time-based conditions and additional fields such as username, destination, or authentication method.

---

# 9. Applying Detection Rules in ELK / Kibana

The same detection concept can be implemented in ELK.

### Typical Workflow

```text
Log Source
    ↓
Elasticsearch
    ↓
Kibana
    ↓
Detection Rule
    ↓
Alert
```

A rule can search authentication logs for repeated failed login events from the same source IP.

---

# 🧪 PRACTICAL EXAMPLES

The following section contains practical detection examples used to demonstrate the concepts above.

---

# 10. Practical Example — Privilege Escalation Detection

Another useful detection is suspicious use of privileged commands.

### Suspicious Command Examples

```bash
sudo su
sudo chmod
sudo rm
```

These commands are not automatically malicious.

Their significance depends on:

- User
- System
- Timing
- Command
- Context
- Authorization

---

## Example 1

```text
User: admin
Command: sudo su
```

---

## Example 2

```text
User: admin
Command: sudo chmod 777 important_file
```

Changing permissions to `777` can be suspicious because it grants excessive permissions.

---

## Example 3

```bash
sudo rm important_file
```

This could indicate destructive or unauthorized activity depending on the context.

### Detection Logic

```text
Privileged Command
       ↓
Suspicious Pattern?
       ↓
Check User / Context
       ↓
Legitimate?
    ↙       ↘
  YES        NO
   ↓          ↓
Exception    Alert
```

---

# 11. Practical Example — Pseudo Code for Detection

A simple conceptual query for repeated suspicious commands can look like:

```sql
SELECT COUNT(*)
FROM sys_logs
WHERE command LIKE "sudo%"
AND timestamp >= NOW() - INTERVAL 30 MINUTE
HAVING COUNT(*) > 2;
```

### Meaning

```text
If a user executes more than 2 sudo commands
within 30 minutes
        ↓
Generate an alert
        ↓
Investigate the activity
```

This is a simplified example of how detection logic can be converted into a query.

---

# 12. Practical Example — Failed Login Detection

### Detection Rule

```text
IF
    failed login attempts >= 5
AND
    attempts originate from the same source IP
AND
    activity occurs within 10 minutes
THEN
    generate alert
```

### Detection Scenario

```text
Failed Login
Failed Login
Failed Login
Failed Login
Failed Login
     ↓
Same Source IP
     ↓
Within 10 Minutes
     ↓
Potential Brute Force
     ↓
Alert
```

---

# 13. Detection Rule Tuning

Creating a rule is not the end of the process.

Detection rules require continuous tuning.

### Tuning Workflow

```text
Create Rule
    ↓
Deploy
    ↓
Monitor Alerts
    ↓
Review False Positives
    ↓
Adjust Conditions
    ↓
Deploy Again
    ↓
Repeat
```

The objective is to create a balance between detection coverage and alert quality.

---

# 14. Threshold Too Low

If the threshold is too low:

```text
Threshold Too Low
       ↓
Too Many Alerts
       ↓
Alert Noise
       ↓
More False Positives
```

### Example

```text
1 failed login → Alert
```

A normal user entering an incorrect password once could generate unnecessary alert noise.

---

# 15. Threshold Too High

If the threshold is too high:

```text
Threshold Too High
       ↓
Fewer Alerts
       ↓
Potential Attack Missed
       ↓
Missed Detection
```

### Example

```text
20 failed logins → Alert
```

An attacker may already perform many attempts before the rule triggers.

Therefore, the threshold should be selected based on realistic user behaviour and the threat being detected.

---

# 16. Exceptions / Allow Lists

Some users, systems, or applications may legitimately perform activities that look suspicious.

### Examples

- Backup services performing sudo commands
- Administrators performing approved maintenance
- Monitoring systems executing automated privileged commands
- Scheduled maintenance scripts

### Example

```text
Backup Service
      ↓
Scheduled sudo commands
```

```text
Admin Account
      ↓
Approved maintenance
```

```text
Monitoring System
      ↓
Automated privileged command
```

Instead of generating alerts for every event, legitimate activity can be added to an exception or allow list where appropriate.

### Exception Logic

```text
Suspicious Activity
       ↓
Check Source / User / Context
       ↓
Legitimate?
    ↙       ↘
  YES        NO
   ↓          ↓
Exception    Alert
```

Exceptions should be carefully controlled because excessive allow-listing can hide real attacks.

---

# 17. False Positives

A false positive occurs when a detection rule generates an alert for legitimate activity.

### Example

```text
Rule:
5 failed logins → Alert
```

```text
Normal user repeatedly enters the wrong password
              ↓
            Alert
              ↓
       False Positive
```

The rule may need tuning based on:

- User behaviour
- Source IP
- Known administrative systems
- Scheduled tasks
- Service accounts
- Business requirements

---

# 18. Monitor False Positives

After deploying a rule, review generated alerts regularly.

### Example

```text
Review False Positive Alerts Weekly
```

If most alerts are legitimate:

- Adjust the threshold
- Improve the pattern
- Add context
- Add carefully controlled exceptions
- Restrict the rule to relevant users or systems

The goal is to reduce unnecessary alerts without allowing attackers to bypass the detection.

---

# 19. Fine-Tune and Repeat

Detection engineering is an iterative process.

```text
Detect
  ↓
Alert
  ↓
Investigate
  ↓
Identify False Positives
  ↓
Tune Rule
  ↓
Deploy
  ↓
Monitor
  ↓
Repeat
```

A detection rule should not simply be created and forgotten.

It should continuously improve based on real-world alerts and analyst feedback.

---

# 20. Detection Engineering Workflow

The complete workflow is:

```text
Identify Behaviour
        ↓
Collect Relevant Logs
        ↓
Define Pattern
        ↓
Set Threshold
        ↓
Define Time Window
        ↓
Create Detection Rule
        ↓
Generate Alert
        ↓
Investigate
        ↓
Review False Positives
        ↓
Add Exceptions if Required
        ↓
Tune Rule
        ↓
Monitor
        ↓
Repeat
```

---

# 21. Key Detection Concepts

| Concept | Meaning |
|---|---|
| Detection Rule | Logic used to identify suspicious behaviour |
| Threshold | Number of events required to trigger detection |
| Pattern | Specific behaviour or event being searched |
| Time Window | Period in which events are evaluated |
| Alert | Notification generated when rule conditions match |
| False Positive | Alert caused by legitimate activity |
| False Negative | Malicious activity that was not detected |
| Exception | Legitimate activity excluded from detection |
| Allow List | Known trusted users, IPs, systems, or activity |
| Rule Tuning | Improving detection accuracy |
| Detection Noise | Large number of low-value alerts |
| Correlation | Combining multiple events to identify suspicious behaviour |

---

# 22. Practical Takeaways

## Start With the Behaviour

Don't start by writing complicated rules.

First ask:

```text
What suspicious behaviour am I trying to detect?
```

---

## Find the Right Logs

A rule is only useful if the required security data exists.

```text
Behaviour
   ↓
Required Logs
   ↓
Detection Logic
```

---

## Define Measurable Conditions

Use:

```text
Pattern + Threshold + Time Window
```

### Example

```text
Failed Login
     +
5 Attempts
     +
10 Minutes
     +
Same Source IP
     ↓
Potential Brute Force
     ↓
Alert
```

---

## Tune Continuously

A detection rule should not be created and forgotten.

Monitor its performance and improve it based on real alerts.

---

# 23. Detection Engineering Mindset

The overall mindset is:

```text
What should I detect?
        ↓
Which logs contain the evidence?
        ↓
What conditions indicate suspicious behaviour?
        ↓
What threshold should trigger an alert?
        ↓
What legitimate activity could cause false positives?
        ↓
How can I tune the rule?
        ↓
Can the rule reliably detect the behaviour?
```

---

# 24. Detection Examples Covered

- [x] Failed login / brute-force detection
- [x] Privilege escalation detection
---

# 25. Future Improvements

The detection rules can later be extended with:

- Suricata IDS rules
- Windows Event Log detections
- Linux authentication detections
- Wazuh rules
- Threat-intelligence enrichment
- MITRE ATT&CK mapping
- Automated alert severity
- Automated response actions
- Detection testing
- Detection-as-code using YAML/JSON
- Automated detection validation
- Alert enrichment
- SOC dashboard integration

---



This structure keeps the project organized and separates actual detection queries/rules from notes and practical examples.

---

# 26. Final Concept

Detection engineering is not just about creating alerts.

It is about creating reliable, actionable detections and continuously improving them based on real-world behaviour.

### Complete Concept

```text
Logs
 ↓
Detection
 ↓
Alert
 ↓
Investigation
 ↓
Tuning
 ↓
Better Detection
```

A good SOC detection should provide useful and actionable alerts without overwhelming the analyst with unnecessary noise.

---

# Final Takeaway

A practical detection engineer follows this cycle:

```text
Identify Behaviour
        ↓
Collect Relevant Logs
        ↓
Define Pattern
        ↓
Set Threshold
        ↓
Define Time Window
        ↓
Create Detection Rule
        ↓
Generate Alert
        ↓
Investigate
        ↓
Review False Positives
        ↓
Add Exceptions if Required
        ↓
Tune Rule
        ↓
Monitor
        ↓
Repeat
```

