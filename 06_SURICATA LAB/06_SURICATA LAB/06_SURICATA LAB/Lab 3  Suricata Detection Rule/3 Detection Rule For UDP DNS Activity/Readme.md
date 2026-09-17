# Lab 03 — UDP Activity Detection

## 🎯 Objective

The objective of this exercise was to create and validate a Suricata detection rule for UDP traffic from Kali Linux to the Windows machine on destination port 53.

The main focus was to understand the complete detection process:

Detection Requirement → Rule → Configuration Test → Suricata → Traffic Generation → Packet Verification → Suricata Telemetry → Alert Investigation

---

## 🖥️ Lab Environment

| Component | Details |
|---|---|
| Traffic Generator | Kali Linux |
| Suricata Sensor / Target | Windows |
| Kali IP | `192.168.56.103` |
| Windows IP | `192.168.56.1` |
| Protocol | UDP |
| Destination Port | `53` |
| Suricata Version | `8.0.6` |
| Rule File | `C:\Suricata\rules\local.rules` |
| Fast Log | `C:\Suricata\log\fast.log` |
| EVE JSON | `C:\Suricata\log\eve.json` |
| Packet Verification | TShark |

---

## 🔎 Detection Requirement

**Requirement:**

> Detect UDP traffic from Kali to Windows on destination port 53.

Before writing the rule, the requirement was broken down into individual traffic characteristics:

    Action            = alert
    Protocol          = UDP
    Source IP         = 192.168.56.103
    Source Port       = any
    Direction         = Kali → Windows
    Destination IP    = 192.168.56.1
    Destination Port  = 53

Expected traffic:

    Kali
    192.168.56.103
          |
          | UDP
          | Destination Port 53
          ↓
    Windows
    192.168.56.1

---

## 🧱 Rule Construction

General Suricata rule structure:

    ACTION PROTOCOL SOURCE_IP SOURCE_PORT -> DESTINATION_IP DESTINATION_PORT (OPTIONS)

The rule was constructed as:

    alert udp 192.168.56.103 any -> 192.168.56.1 53

The rule options were then added:

    msg:"UDP_ACTIVITY"
    sid:1000004
    rev:1

### Final Rule

    alert udp 192.168.56.103 any -> 192.168.56.1 53 (msg:"UDP_ACTIVITY"; sid:1000004; rev:1;)

### Rule Meaning

    alert
    → Generate an alert when the traffic matches.

    udp
    → Detect UDP traffic.

    192.168.56.103
    → Kali is the source.

    any
    → Any source port is allowed.

    ->
    → Traffic must flow from Kali to Windows.

    192.168.56.1
    → Windows is the destination.

    53
    → Destination port must be 53.

    UDP_ACTIVITY
    → Human-readable detection name.

    1000004
    → Unique SID for this custom rule.

    rev:1
    → Rule revision.

---

## 📁 Add the Rule

The rule was added to:

    C:\Suricata\rules\local.rules

Final entry:

    alert udp 192.168.56.103 any -> 192.168.56.1 53 (msg:"UDP_ACTIVITY"; sid:1000004; rev:1;)

### 📸 Screenshot

    Images/01_udp_activity_rule.png

![UDP Activity Rule](Images/01_udp_activity_rule.png)

---

## 🧪 Configuration Test

Before starting Suricata, the configuration was tested:

    cd /d C:\Suricata

    suricata.exe -T -c C:\Suricata\suricata.yaml

The configuration and rule loaded successfully.

Important output:

    rules successfully loaded

    Configuration provided was successfully loaded

### 📸 Screenshot

    Images/02_udp_rule_test.png

![UDP Rule Test](Images/02_udp_rule_test.png)

---

## ▶️ Start Suricata

Suricata was started using:

    Start-Suricata.bat

The engine successfully displayed:

    Engine started

The Suricata window was kept running while traffic was generated from Kali.

### 📸 Screenshot

    Images/03_suricata_engine_started.png

![Suricata Engine Started](Images/03_suricata_engine_started.png)

---

## 📡 Generate UDP Traffic

The following command was used from Kali:

    echo "test" | nc -u -w 1 192.168.56.1 53

### Command Breakdown

    echo "test"
    → Creates test data.

    |
    → Sends the data to Netcat.

    nc
    → Netcat is used to generate the network traffic.

    -u
    → Uses UDP.

    -w 1
    → Uses a short timeout.

    192.168.56.1
    → Windows destination.

    53
    → Destination port.

Traffic generated:

    192.168.56.103:any
            |
            | UDP
            ↓
    192.168.56.1:53

### 📸 Screenshot

    Images/04_kali_udp_traffic.png

![Kali UDP Traffic](Images/04_kali_udp_traffic.png)

---

## 🔬 UDP Traffic Verification

Initially, the expected UDP alert was not immediately visible in `fast.log`.

Instead of immediately changing the rule, the traffic path was investigated.

TShark interfaces were listed using:

    & "C:\Program Files\Wireshark\tshark.exe" -D

The Windows adapter containing:

    192.168.56.1

was identified.

The corresponding TShark interface was used to capture:

    udp port 53

TShark successfully showed:

    192.168.56.103 → 192.168.56.1
    UDP
    source port → 53

This proved that the UDP packet was actually reaching the Windows interface.

### 📸 Screenshot

    Images/05_tshark_udp_capture.png

![TShark UDP Capture](Images/05_tshark_udp_capture.png)

---

## 🔎 Suricata Telemetry Verification

After confirming the packet with TShark, Suricata's EVE JSON was checked.

Command:

    Get-Content C:\Suricata\log\eve.json -Tail 100 | Select-String "192.168.56.103"

This confirmed that Suricata was also seeing the UDP traffic.

Investigation chain:

    Kali generates UDP traffic
            ↓
    Windows interface receives packet
            ↓
    TShark sees packet
            ↓
    Suricata sees packet
            ↓
    Detection rule processes traffic
            ↓
    Alert can be investigated

### 📸 Screenshot

    Images/06_eve_json_udp.png

![EVE JSON UDP](Images/06_eve_json_udp.png)

---

## 🚨 Alert Verification

The Suricata alert log was checked using:

    type C:\Suricata\log\fast.log

The detection name used for this rule was:

    UDP_ACTIVITY

The EVE JSON alert was used to investigate:

    timestamp
    src_ip
    src_port
    dest_ip
    dest_port
    proto
    alert

### 📸 Screenshot

    Images/07_udp_activity_alert.png

![UDP Activity Alert](Images/07_udp_activity_alert.png)

---

## 🎯 Rule Specificity Test

The rule was specifically designed to detect:

    UDP
    192.168.56.103
            ↓
    192.168.56.1:53

A different UDP destination port was tested:

    echo "test" | nc -u -w 1 192.168.56.1 9999

This traffic was sent to port `9999` instead of port `53`.

Therefore, it does not satisfy the destination-port condition of the `UDP_ACTIVITY` rule.

This demonstrated that the rule is not simply detecting all UDP traffic.

It is detecting a specific traffic pattern:

    UDP
    +
    Kali source
    +
    Windows destination
    +
    Destination port 53

### 📸 Screenshot

    Images/08_udp_specificity_test.png

![UDP Specificity Test](Images/08_udp_specificity_test.png)

---

## 🕵️ SOC Investigation

An alert does not automatically mean an attack.

The investigation process learned in this exercise was:

    ALERT
      ↓
    Check timestamp
      ↓
    Identify source IP
      ↓
    Identify destination IP
      ↓
    Identify protocol
      ↓
    Identify source and destination ports
      ↓
    Understand why the rule matched
      ↓
    Check context
      ↓
    Classify the event

For this lab, the traffic was intentionally generated by us.

Therefore:

    Alert      = Detection triggered
    Activity   = Controlled lab traffic
    Context    = Lab testing
    Result     = Benign / Lab Test

### Important Concept

    ALERT ≠ ATTACK

A detection rule only tells us that traffic matched the conditions defined in the rule.

The SOC analyst must investigate the context before deciding whether the activity is suspicious or malicious.

---

## 📚 What We Learned

1. Start with the detection requirement instead of immediately writing syntax.

2. Break the requirement into:

       Protocol
       Source
       Source Port
       Direction
       Destination
       Destination Port
       Action

3. Use `any` when a specific IP or port is not required.

4. Use a specific port when the detection requirement is focused on a particular service.

5. `msg` gives the alert a readable detection name.

6. `sid` uniquely identifies the detection rule.

7. `rev` identifies the rule revision.

8. Use `-T` to test the Suricata configuration before running it.

9. Use TShark to confirm whether the actual packet reached the monitored interface.

10. Use `fast.log` for quick alert verification.

11. Use `eve.json` for structured investigation data.

12. Do not immediately blame the detection rule when an alert does not appear. Trace the traffic path first.

13. A Suricata alert requires investigation and context.

---

## 🆕 New Learning From This Exercise

### 1. Detection Requirement → Traffic Characteristics → Rule

The biggest new concept was learning to convert a security requirement into a concrete traffic pattern and then into a Suricata rule.

Example:

    "Detect UDP traffic from Kali to Windows on port 53."

becomes:

    UDP
    192.168.56.103:any
            ↓
    192.168.56.1:53

which becomes:

    alert udp 192.168.56.103 any -> 192.168.56.1 53 (msg:"UDP_ACTIVITY"; sid:1000004; rev:1;)

---

### 2. Rule Specificity

A broad rule:

    alert udp any any -> any any

can match a large amount of UDP traffic.

A more specific rule:

    alert udp 192.168.56.103 any -> 192.168.56.1 53 (msg:"UDP_ACTIVITY"; sid:1000004; rev:1;)

matches a much more defined traffic pattern.

The important lesson is:

    More specific requirement
            ↓
    More specific detection

---

### 3. Troubleshooting by Layers

When the alert did not immediately appear, we did not randomly modify the rule.

We checked:

    Traffic generated?
            ↓
    Packet reached Windows?
            ↓
    TShark saw it?
            ↓
    Suricata saw it?
            ↓
    Rule matched?
            ↓
    Alert generated?

This is the basic troubleshooting mindset we will continue using in future SOC detection labs.

---

## 🔄 Final Detection Workflow

    REQUIREMENT
         ↓
    BREAK DOWN TRAFFIC
         ↓
    BUILD RULE
         ↓
    SAVE local.rules
         ↓
    TEST CONFIGURATION
         ↓
    START SURICATA
         ↓
    GENERATE MATCHING TRAFFIC
         ↓
    VERIFY WITH TSHARK
         ↓
    VERIFY SURICATA TELEMETRY
         ↓
    CHECK fast.log
         ↓
    INVESTIGATE eve.json
         ↓
    CLASSIFY EVENT

---

## ✅ Lab Status

    UDP Detection Rule       = COMPLETED
    Rule Configuration Test  = COMPLETED
    UDP Traffic Generation   = COMPLETED
    TShark Verification      = COMPLETED
    Suricata Verification    = COMPLETED
    Alert Investigation      = COMPLETED
    Rule Specificity Test    = COMPLETED

---

## 🏁 Final Takeaway

The important skill from this exercise is not memorizing the `UDP_ACTIVITY` rule.

The important skill is:

    Detection Requirement
            ↓
    Understand What Traffic Represents It
            ↓
    Build Detection
            ↓
    Generate Evidence
            ↓
    Verify
            ↓
    Investigate
            ↓
    Classify


```
