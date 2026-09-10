#  First Suricata Detection Rule

## 🎯 Objective

The objective of this lab was to understand how to create a Suricata
detection rule from a simple detection requirement and then verify
that the rule actually works.

The main focus was not memorizing Suricata syntax.

The focus was understanding the detection-engineering mindset:

    Detection Requirement
            ↓
    Understand the Traffic
            ↓
    Build Rule Logic
            ↓
    Write Suricata Rule
            ↓
    Test Rule
            ↓
    Generate Controlled Traffic
            ↓
    Verify Alert
            ↓
    Investigate Alert

---

# 🏗️ Lab Environment

## Kali Linux

IP Address:

    192.168.56.103

Used to generate controlled network traffic.

---

## Windows

IP Address:

    192.168.56.1

Windows acts as the Suricata monitoring system.

---

## Suricata

Installation:

    C:\Suricata\

Configuration:

    C:\Suricata\suricata.yaml

Rules:

    C:\Suricata\rules\local.rules

Logs:

    C:\Suricata\log\

EVE JSON:

    C:\Suricata\log\eve.json

Fast Log:

    C:\Suricata\log\fast.log

---

# 🔄 Complete Detection Architecture

    Kali Linux
    192.168.56.103
          |
          | ICMP Traffic
          v
    Host-Only Network
          |
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
    Detection Rules
          |
          v
       Rule Match
          |
          v
        ALERT
          |
       +--+--+
       |     |
       v     v
    fast.log eve.json
                |
                v
           SOC Analyst
                |
                v
           Investigation

---

# 🧠 1. What Is a Suricata Detection Rule?

A Suricata detection rule is a condition that tells Suricata:

    "If network traffic matches these conditions,
     perform a specific action."

For example:

    "If ICMP traffic is observed,
     generate an alert."

The basic detection process is:

    Network Traffic
          ↓
    Suricata receives traffic
          ↓
    Compare traffic with rules
          ↓
    Does the traffic match?
          ↓
       YES / NO
          ↓
       YES
          ↓
       ALERT

---

# 🎯 2. Start With a Detection Requirement

We learned that detection rules should not be created by blindly
memorizing syntax.

First define the detection requirement.

Our requirement was:

    "Generate an alert when ICMP traffic is observed."

Then we translate this requirement into rule logic.

Ask:

    What action?
    What protocol?
    Who is the source?
    What is the source port?
    What direction?
    Who is the destination?
    What is the destination port?
    What message should be shown?
    What SID identifies the rule?
    What revision is this?

---

# 🧩 3. Basic Suricata Rule Structure

A Suricata rule contains two major parts:

    RULE HEADER
          +
    RULE OPTIONS

Example:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

The rule can be divided into:

    alert icmp any any -> any any
    ─────────────────────────────
             RULE HEADER

    msg:"LAB3_ICMP_ACTIVITY";
    sid:1000002;
    rev:1;
    ─────────────────────────────
             RULE OPTIONS

---

# 🧱 4. Rule Header

The basic structure is:

    ACTION PROTOCOL SOURCE_IP SOURCE_PORT DIRECTION DEST_IP DEST_PORT

Example:

    alert icmp any any -> any any

The header answers:

    WHAT ACTION?
         ↓
    WHICH PROTOCOL?
         ↓
    FROM WHOM?
         ↓
    FROM WHICH PORT?
         ↓
    IN WHICH DIRECTION?
         ↓
    TO WHOM?
         ↓
    TO WHICH PORT?

---

# 5. `alert`

Example:

    alert

Meaning:

    "Generate an alert when the traffic matches the rule."

Concept:

    Traffic
       ↓
    Rule Match
       ↓
    ALERT

For our SOC IDS lab, `alert` is the main action being used.

---

# 6. Protocol

Example:

    icmp

This tells Suricata which protocol the rule should inspect.

Therefore:

    alert icmp

means:

    Generate an alert for matching ICMP traffic.

Other protocols that can be used in rules include:

    tcp
    udp
    icmp

The important question is:

    "What protocol am I trying to detect?"

---

# 7. Source IP

Example:

    any

`any` means:

    Any source IP

We could also specify a particular source:

    192.168.56.103

That would make the rule specific to the Kali machine.

Concept:

    any
      ↓
    Any source

    192.168.56.103
      ↓
    Specific source

---

# 8. Source Port

Example:

    any

This represents the source port.

Therefore:

    alert icmp any any

can be understood as:

    Protocol    = ICMP
    Source IP   = Any
    Source Port = Any

ICMP does not use ports in the same way as TCP and UDP.

However, understanding the source-port position is important because
TCP and UDP detection rules commonly use ports.

---

# 9. Direction — `->`

The symbol:

    ->

represents traffic direction.

The concept is:

    SOURCE -> DESTINATION

Our lab traffic:

    192.168.56.103 -> 192.168.56.1

means:

    Kali → Windows

Direction is important because many detections depend on who is
communicating with whom.

---

# 10. Destination IP

Example:

    any

This represents the destination IP.

`any` means:

    Any destination IP

A specific destination could also be used:

    192.168.56.1

This would restrict the destination to the Windows machine.

---

# 11. Destination Port

Example:

    any

This represents the destination port.

So:

    alert icmp any any -> any any

means:

    Generate an alert when ICMP traffic is observed
    from any source to any destination.

---

# 🧩 12. Rule Options

The rule options provide additional information about the detection.

Our rule uses:

    msg
    sid
    rev

Example:

    (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

---

# 13. `msg`

Example:

    msg:"LAB3_ICMP_ACTIVITY";

`msg` defines the message/name associated with the detection.

Think:

    "What should the analyst see when this rule fires?"

A useful message should describe what the detection is intended
to identify.

---

# 14. `sid`

Example:

    sid:1000002;

SID identifies the rule.

Think:

    SID = Rule ID

Example:

    1000001 → Rule 1
    1000002 → Rule 2
    1000003 → Rule 3

The SID allows analysts to identify the specific rule that
generated an alert.

---

# 15. `rev`

Example:

    rev:1;

`rev` represents the revision/version of the rule.

Example:

    rev:1
       ↓
    First version

If the rule is modified:

    rev:2
       ↓
    Second version

This allows changes to detection rules to be tracked.

---

# 🧠 16. Building a Rule From a Requirement

Our detection requirement:

    "Generate an alert when ICMP traffic is observed."

We translated it step-by-step.

### Step 1 — Action

    alert

### Step 2 — Protocol

    icmp

### Step 3 — Source IP

    any

### Step 4 — Source Port

    any

### Step 5 — Direction

    ->

### Step 6 — Destination IP

    any

### Step 7 — Destination Port

    any

This gives:

    alert icmp any any -> any any

Now add the rule options.

### Message

    msg:"LAB3_ICMP_ACTIVITY";

### SID

    sid:1000002;

### Revision

    rev:1;

Final rule:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

---

# 🔍 17. Reading the Complete Rule

Rule:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

Read it as:

    alert
    → Generate an alert

    icmp
    → Look for ICMP traffic

    any any
    → Any source IP and source port

    ->
    → Traffic moving from source to destination

    any any
    → Any destination IP and destination port

    msg:"LAB3_ICMP_ACTIVITY"
    → Name/message of the detection

    sid:1000002
    → Unique rule ID

    rev:1
    → Rule version 1

Complete meaning:

    "Generate an alert when ICMP traffic is observed
     from any source to any destination and identify
     the detection as LAB3_ICMP_ACTIVITY."

---

# 📝 18. Add the Rule to local.rules

Rule file:

    C:\Suricata\rules\local.rules

The new rule added was:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

We kept the previously created ICMP rule as well:

    alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)

The second rule used a different SID:

    1000002

This prevents the new rule from using the same rule identifier.

---

# 🧪 19. Test the Suricata Configuration

After creating the rule, we did NOT immediately assume that it worked.

First, we tested the Suricata configuration.

Command:

    suricata.exe -T -c C:\Suricata\suricata.yaml

Meaning:

    suricata.exe
        ↓
    Run Suricata

    -T
        ↓
    Test configuration

    -c
        ↓
    Use configuration file

    C:\Suricata\suricata.yaml
        ↓
    Suricata configuration

---

# ✅ 20. Configuration Test Result

Suricata successfully processed the rule.

Important output:

    1 rule files processed.
    1 rules successfully loaded.
    0 rules failed.
    0 rules skipped.

Suricata also reported:

    1 signatures processed

And finally:

    Configuration provided was successfully loaded. Exiting.

### Conclusion

The rule syntax was accepted by Suricata.

Therefore:

    Rule Created
         ↓
    Configuration Tested
         ↓
    Rule Successfully Loaded

---

# ⚠️ 21. Warning Observed During Configuration Test

A warning related to:

    threshold.config

was displayed during the test.

However, Suricata still successfully loaded the configuration and
our rule.

The important result for this step was:

    1 rules successfully loaded
    0 rules failed

Therefore, the warning did not prevent our detection rule from
being loaded or tested.

---

# 🚀 22. Start Suricata

After the configuration test passed, Suricata was started normally.

Command:

    cd /d C:\Suricata

Then:

    suricata.exe -c C:\Suricata\suricata.yaml -i 192.168.56.1

The engine displayed:

    Engine started

This confirmed that Suricata was actively monitoring the interface.

The Suricata window was kept running while traffic was generated
from Kali.

---

# 📡 23. Generate Controlled Traffic

From Kali Linux:

    ping 192.168.56.1

The ping was allowed to run for several seconds and then stopped:

    Ctrl + C

Traffic flow:

    Kali
    192.168.56.103
          |
          | ICMP
          v
    Windows
    192.168.56.1

Our rule was specifically looking for:

    ICMP

Therefore, we expected the rule to match.

---

# 🔎 24. Predict Before Checking

Before checking the logs, we predicted:

    ICMP Traffic
          ↓
    Suricata receives it
          ↓
    Rule conditions match
          ↓
    LAB3_ICMP_ACTIVITY
          ↓
    ALERT

This prediction step is important because detection engineering
is not simply:

    Run command → Look at result

Instead:

    Understand detection
         ↓
    Predict expected behavior
         ↓
    Test
         ↓
    Compare actual vs expected

---

# 🚨 25. Check fast.log

After generating the traffic, we checked:

    C:\Suricata\log\fast.log

Command:

    type C:\Suricata\log\fast.log

The new detection was observed:

    LAB3_ICMP_ACTIVITY

with the corresponding SID:

    1000002

### Conclusion

Our rule successfully detected the ICMP traffic.

---

# 🔄 26. Detection Chain

The complete detection chain was:

    Kali
      ↓
    ICMP Traffic
      ↓
    Npcap
      ↓
    Suricata
      ↓
    local.rules
      ↓
    ICMP rule
      ↓
    Rule Match
      ↓
    LAB3_ICMP_ACTIVITY
      ↓
    Alert
      ↓
    fast.log

This proved that the rule was not only syntactically valid but
also operational.

---

# 📄 27. Verify the Alert in EVE JSON

After confirming the alert in `fast.log`, we investigated the
structured EVE JSON telemetry.

File:

    C:\Suricata\log\eve.json

While Suricata was running, directly trying to open the file as
a command produced:

    The process cannot access the file because it is being used
    by another process.

This happened because Suricata was actively using the file for
logging.

The correct approach was to read the file from another PowerShell
window.

---

# 🖥️ 28. Read Recent EVE JSON Events

Instead of dumping the entire file, we used:

    Get-Content C:\Suricata\log\eve.json -Tail 20

### Why `-Tail 20`?

EVE JSON can become very large.

Instead of reading everything:

    Get-Content
          ↓
    Read eve.json

    -Tail 20
          ↓
    Show the latest 20 lines

This is more practical when working with continuously growing logs.

---

# 🧩 29. Parse EVE JSON

To convert JSON text into PowerShell objects:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json }

Concept:

    JSON Text
        ↓
    ConvertFrom-Json
        ↓
    PowerShell Objects
        ↓
    Fields Become Accessible

---

# 🔎 30. Filter Only Alert Events

We then filtered for:

    event_type = alert

Command:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json } | Where-Object {$_.event_type -eq "alert"}

The process was:

    eve.json
       ↓
    Read
       ↓
    Parse JSON
       ↓
    Check event_type
       ↓
    Keep only alert events

---

# 📊 31. Extract Useful Alert Fields

After filtering the alerts, we extracted useful fields:

    timestamp
    src_ip
    src_port
    dest_ip
    dest_port
    proto
    alert

Command:

    Get-Content C:\Suricata\log\eve.json | ForEach-Object { $_ | ConvertFrom-Json } | Where-Object {$_.event_type -eq "alert"} | Select-Object timestamp,src_ip,src_port,dest_ip,dest_port,proto,alert | Format-Table -AutoSize

---

# 🧠 32. Understanding the Extraction Pipeline

The command is not something to memorize as one huge command.

It is a sequence:

    Get-Content
        ↓
    Read the log

    ForEach-Object
        ↓
    Process each JSON line

    ConvertFrom-Json
        ↓
    Convert JSON into objects

    Where-Object
        ↓
    Keep only alert events

    Select-Object
        ↓
    Extract useful fields

    Format-Table
        ↓
    Make output readable

The actual methodology is:

    READ
      ↓
    PARSE
      ↓
    FILTER
      ↓
    EXTRACT
      ↓
    DISPLAY
      ↓
    INVESTIGATE

---

# 🔬 33. SOC Investigation of the Alert

Once an alert is found, the analyst should ask:

    WHO?
    WHAT?
    WHEN?
    WHERE?
    WHY?
    IS IT MALICIOUS?

For our controlled alert:

### WHO?

Source:

    192.168.56.103

This was the Kali machine.

---

### WHERE?

Destination:

    192.168.56.1

This was the Windows machine.

---

### WHAT?

Protocol:

    ICMP

---

### WHEN?

Use:

    timestamp

from the EVE JSON alert.

---

### WHY?

The traffic matched:

    LAB3_ICMP_ACTIVITY

Rule SID:

    1000002

---

### IS IT MALICIOUS?

No.

The ICMP traffic was intentionally generated as part of the
controlled SOC Home Lab.

---

# 🚨 34. Important SOC Concept — Alert ≠ Attack

One of the most important lessons from this lab:

    ALERT ≠ ATTACK

A detection rule means:

    "This activity matched a condition
     that we decided to detect."

It does not automatically mean:

    "An attacker is present."

The analyst must investigate.

Correct methodology:

    ALERT
      ↓
    INVESTIGATION
      ↓
    CONTEXT
      ↓
    CLASSIFICATION

Possible classifications:

    Benign Activity
    False Positive
    Suspicious Activity
    True Positive

Our lab example:

    Alert
      ↓
    Controlled ICMP
      ↓
    Known Lab Activity
      ↓
    Benign

---

# 🧠 35. Detection Engineering Mindset

The main skill developed in this lab is:

    Do not start with syntax.

Start with:

    "What behavior do I want to detect?"

Then:

    Detection Requirement
          ↓
    Traffic Characteristics
          ↓
    Rule Logic
          ↓
    Suricata Syntax
          ↓
    Configuration Test
          ↓
    Controlled Traffic
          ↓
    Alert Verification
          ↓
    Investigation
          ↓
    Detection Improvement

---

# 🔥 36. What We Actually Proved

We did not simply create a text line in `local.rules`.

We proved the complete detection lifecycle.

    1. Detection requirement created

    2. Rule logic designed

    3. Rule written

    4. Rule saved in local.rules

    5. Suricata configuration tested

    6. Rule successfully loaded

    7. Suricata started

    8. Controlled ICMP generated

    9. Rule matched the traffic

    10. Alert appeared in fast.log

    11. Alert was found in EVE JSON

    12. Useful fields were extracted

    13. Alert was investigated

    14. Activity was classified as benign
        controlled lab traffic

---

# 📸 37. Evidence / Screenshots

## Screenshot 01 — Suricata Engine Started

Shows Suricata running successfully.

    Images/01_suricata_engine_started.png

![Suricata Engine Started](Images/01_suricata_engine_started.png)


---

## Screenshot 02 — Kali ICMP Traffic

Shows Kali generating controlled ICMP traffic toward Windows.

    Images/02_kali_icmp.png

![Kali ICMP Traffic](Images/02_kali_icmp.png)


---

## Screenshot 03 — Rule in local.rules

Shows the created detection rule:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

    Images/03_lab3_detection_rule.png

![Lab 03 Detection Rule](Images/03_lab3_detection_rule.png)


---

## Screenshot 04 — Configuration Test

Shows that the rule was successfully loaded.

    Images/04_rule_test.png

![Rule Configuration Test](Images/04_rule_test.png)


---

## Screenshot 05 — fast.log Alert

Shows:

    LAB3_ICMP_ACTIVITY

and:

    SID:1000002

    Images/05_lab3_fastlog_alert.png

![Lab 03 Alert](Images/05_lab3_fastlog_alert.png)


---

## Screenshot 06 — EVE JSON Alert

Shows the alert inside:

    C:\Suricata\log\eve.json

    Images/06_eve_json_alert.png

![EVE JSON Alert](Images/06_eve_json_alert.png)


---

## Screenshot 07 — PowerShell Alert Extraction

Shows the extracted fields:

    timestamp
    src_ip
    src_port
    dest_ip
    dest_port
    proto
    alert

    Images/07_powershell_alert_extraction.png

![PowerShell Alert Extraction](Images/07_powershell_alert_extraction.png)


---

# 📚 38. Important Terms

## Detection Rule

A condition that tells Suricata what traffic or behavior to detect.

---

## Rule Header

Defines the basic traffic characteristics:

    Action
    Protocol
    Source
    Source Port
    Direction
    Destination
    Destination Port

---

## Rule Options

Provide additional information and control:

    msg
    sid
    rev

---

## SID

Unique identifier of a Suricata rule.

    SID = Rule ID

---

## Revision

Version number of the rule.

    rev:1
    rev:2
    rev:3

---

## `msg`

Human-readable detection message.

---

## Rule Match

When observed traffic satisfies the conditions defined by the rule.

---

## Alert

Generated when an `alert` rule matches traffic.

---

# 🧠 39. Quick Revision Sheet

## Rule Structure

    ACTION PROTOCOL SOURCE SOURCE_PORT -> DESTINATION DEST_PORT (OPTIONS)

---

## Example

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

---

## Meaning

    alert
    → Generate alert

    icmp
    → Protocol

    any
    → Any source

    any
    → Any source port

    ->
    → Direction

    any
    → Any destination

    any
    → Any destination port

    msg
    → Detection message

    sid
    → Rule ID

    rev
    → Rule version

---

# 🔑 40. How To Create Any Basic Rule

Whenever you are given a detection requirement, ask:

    1. WHAT DO I WANT TO DETECT?
              ↓
    2. WHICH PROTOCOL?
              ↓
    3. WHO IS THE SOURCE?
              ↓
    4. WHICH SOURCE PORT?
              ↓
    5. WHAT DIRECTION?
              ↓
    6. WHO IS THE DESTINATION?
              ↓
    7. WHICH DESTINATION PORT?
              ↓
    8. WHAT MESSAGE?
              ↓
    9. WHICH SID?
              ↓
    10. WHICH REVISION?

Then convert those answers into Suricata syntax.

---

# 🔄 41. Detection Workflow To Remember

    Detection Idea
          ↓
    Define Traffic
          ↓
    Build Rule
          ↓
    Save Rule
          ↓
    Test Configuration
          ↓
    Start Suricata
          ↓
    Generate Traffic
          ↓
    Check fast.log
          ↓
    Check eve.json
          ↓
    Extract Evidence
          ↓
    Investigate
          ↓
    Classify Activity

---

# 🎯 42. Key Lessons

### Lesson 1

A detection rule starts with a **detection requirement**, not syntax.

---

### Lesson 2

A rule has:

    Header + Options

---

### Lesson 3

The header describes the traffic.

---

### Lesson 4

The options provide identification and additional information.

---

### Lesson 5

A rule should always be tested before assuming it works.

---

### Lesson 6

A successful configuration test proves that Suricata accepted
the rule.

It does NOT prove that the rule actually detects the intended
traffic.

For that, we need:

    Real/Controlled Traffic
          ↓
    Rule Match
          ↓
    Alert

---

### Lesson 7

`fast.log` provides a quick alert view.

`eve.json` provides structured telemetry that can be processed
and investigated.

---

### Lesson 8

A SOC analyst should extract useful evidence rather than reading
the entire raw log.

---

### Lesson 9

An alert is a starting point for investigation.

    ALERT ≠ ATTACK

---

# 🧠 Final Mental Model

The entire Lab 03 concept can be remembered as:

    WHAT DO I WANT TO DETECT?
              ↓
        DEFINE CONDITIONS
              ↓
          WRITE RULE
              ↓
          TEST RULE
              ↓
        GENERATE TRAFFIC
              ↓
          RULE MATCHES
              ↓
             ALERT
              ↓
          CHECK LOGS
              ↓
       EXTRACT EVIDENCE
              ↓
        INVESTIGATE ALERT
              ↓
          ADD CONTEXT
              ↓
       CLASSIFY ACTIVITY

---


# 🚀 Final Takeaway

The most important thing learned in this lab was not the following
rule:

    alert icmp any any -> any any (msg:"LAB3_ICMP_ACTIVITY"; sid:1000002; rev:1;)

The important skill is being able to think:

    "I want to detect this behavior."

Then:

    What traffic represents that behavior?
             ↓
    What conditions should match?
             ↓
    How do I express those conditions in Suricata?
             ↓
    How do I test the rule?
             ↓
    How do I verify the alert?
             ↓
    How do I investigate the alert?

That is the foundation of **SOC detection engineering**.

    DETECTION IDEA
         ↓
    RULE LOGIC
         ↓
    SURICATA
         ↓
    ALERT
         ↓
    INVESTIGATION