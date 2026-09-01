# SOC Home Lab — Lab 01: Suricata Installation & Setup

## 🎯 Objective

Install and configure Suricata IDS on Windows, connect it to the correct network interface, create a custom detection rule, generate traffic from Kali Linux, and verify the resulting alerts in `fast.log` and `eve.json`.

---

## 🏗️ Lab Architecture

Kali Linux (Attacker)
- IP: `192.168.56.103`

        ↓ ICMP Traffic

Host-Only Network
- Network: `192.168.56.0/24`

        ↓

Windows Host (Suricata IDS)
- IP: `192.168.56.1`
- Interface: `Ethernet`

Traffic Flow:

Kali → Npcap → Suricata → Detection Rule → Alert → Logs

---

## 🧰 Tools Used

- Windows 11
- Kali Linux
- Suricata 8.0.6
- Npcap
- Wireshark / TShark
- Command Prompt
- PowerShell

---

## 📁 Important Suricata Files

Configuration:
`C:\Suricata\suricata.yaml`

Rules:
`C:\Suricata\rules\local.rules`

Logs:
`C:\Suricata\log\`

Main logs:

`fast.log` → Human-readable alerts

`eve.json` → Structured JSON events and alerts

---

## ⚙️ Suricata Configuration

Important network configuration:

HOME_NET:
`192.168.0.0/16`

EXTERNAL_NET:
`!$HOME_NET`

The Host-Only network `192.168.56.0/24` falls inside the configured HOME_NET range.

Rule file:

`local.rules`

---

## 🚨 Custom Detection Rule

Rule used:

`alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)`

Rule breakdown:

- `alert` → Generate an alert
- `icmp` → Detect ICMP traffic
- `any any` → Any source IP/port
- `->` → Traffic direction
- `any any` → Any destination IP/port
- `msg:"ICMP_TEST"` → Alert message
- `sid:1000001` → Unique rule ID
- `rev:1` → Rule revision

Purpose:

Detect ICMP traffic between Kali and Windows.

---

## ✅ Step 1 — Test Suricata Configuration

Run:

`cd /d C:\Suricata`

Then:

`suricata.exe -T -c C:\Suricata\suricata.yaml`

Successful output should show that the configuration was successfully loaded and the rules were successfully processed.

---

## 🔌 Step 2 — Find Network Interfaces

Run:

`"C:\Program Files\Wireshark\tshark.exe" -D`

This displays the available Npcap interfaces.

Example:

`8. \Device\NPF_{...} (Ethernet)`

The Npcap device is required by Suricata to capture Windows network traffic.

---

## 🔎 Step 3 — Identify the Correct Windows Interface

Windows Host-Only IP:

`192.168.56.1`

Check its interface:

`powershell -Command "Get-NetIPAddress -IPAddress 192.168.56.1 | Format-List IPAddress,InterfaceAlias,InterfaceIndex"`

Result:

`IPAddress       : 192.168.56.1`
`InterfaceAlias  : Ethernet`
`InterfaceIndex  : 11`

Important:

The Windows InterfaceIndex (`11`) is not necessarily the same as the TShark capture interface number.

Therefore, use TShark `-D` to identify the actual Npcap device.

---

## 📡 Step 4 — Verify Traffic with TShark

Before starting Suricata, verify that packets are reaching the correct interface.

Run:

`"C:\Program Files\Wireshark\tshark.exe" -i 8 -f "icmp"`

Then from Kali:

`ping 192.168.56.1`

Expected traffic:

`192.168.56.103 → 192.168.56.1`

and:

`192.168.56.1 → 192.168.56.103`

If TShark displays these packets, packet capture is working correctly.

---

## 🛡️ Step 5 — Start Suricata

Use the actual Npcap device discovered with TShark.

Example:

`cd /d C:\Suricata`

`suricata.exe -c C:\Suricata\suricata.yaml -i "\Device\NPF_{60E3C755-D7C8-45DE-9630-91666505DF7AC}"`

When successfully started, Suricata displays:

`Engine started.`

Keep this CMD window running.

IMPORTANT:

While Suricata is running, the CMD window is occupied by Suricata.

Open another Administrator CMD to check logs.

---

## 💥 Step 6 — Generate Test Traffic

From Kali Linux:

`ping -c 10 192.168.56.1`

This generates ICMP packets.

Because our custom rule detects ICMP traffic, Suricata should trigger:

`ICMP_TEST`

---

## 📄 Step 7 — Check fast.log

Open another CMD and run:

`findstr /i "ICMP_TEST" C:\Suricata\log\fast.log`

Expected result contains:

`[1:1000001:1]`

and:

`ICMP`

and:

`192.168.56.103 -> 192.168.56.1`

This confirms that Suricata generated the human-readable alert.

---

## 📊 Step 8 — Check eve.json

Run:

`findstr /i "ICMP_TEST" C:\Suricata\log\eve.json`

If the rule triggered successfully, the JSON log contains the `ICMP_TEST` alert.

`eve.json` provides structured information such as:

- Timestamp
- Source IP
- Destination IP
- Protocol
- Alert
- Signature
- Signature ID
- Severity

---

## 🧠 How the Complete System Works

The complete process is:

Kali generates ICMP traffic
        ↓
Windows Host-Only adapter receives traffic
        ↓
Npcap captures the packets
        ↓
Suricata reads the packets
        ↓
Suricata analyzes the traffic
        ↓
Detection engine checks `local.rules`
        ↓
ICMP rule matches
        ↓
Suricata generates an alert
        ↓
Alert is written to:
        ↓
`fast.log`
        +
`eve.json`

---

## 🔥 SOC Troubleshooting Methodology

If an alert does not appear, troubleshoot layer by layer:

1. Is Kali generating traffic?
2. Is Windows receiving the traffic?
3. Is Npcap capturing the traffic?
4. Can TShark see the packets?
5. Is Suricata listening on the correct Npcap interface?
6. Did Suricata successfully load the configuration?
7. Did Suricata successfully load the rule?
8. Does the rule match the traffic?
9. Is the alert being written to `fast.log`?
10. Is the alert being written to `eve.json`?

This prevents guessing and follows a proper SOC troubleshooting methodology.

---

## 🔄 Daily Startup Procedure

Every day, the basic workflow is:

### CMD 1 — Start Suricata

`cd /d C:\Suricata`

`suricata.exe -c C:\Suricata\suricata.yaml -i "\Device\NPF_{60E3C755-D7C8-45DE-9630-91666505DF7AC}"`

Keep it running.

### Kali — Generate Traffic

`ping -c 10 192.168.56.1`

### CMD 2 — Check Alert

`findstr /i "ICMP_TEST" C:\Suricata\log\fast.log`

or:

`findstr /i "ICMP_TEST" C:\Suricata\log\eve.json`

---

## 🧩 Problems Encountered & Fixed

### 1. Missing Rule File

Error:

`No such file or directory`

Cause:

Suricata configuration referenced a rule file that was not available.

Solution:

Corrected the rule configuration and used the available local rule file.

### 2. HOME_NET / EXTERNAL_NET Error

Error:

`address var - "EXTERNAL_NET" has the complete IP space negated`

Cause:

Incorrect YAML configuration/indentation.

Solution:

Corrected:

`HOME_NET: 192.168.0.0/16`

`EXTERNAL_NET: "!$HOME_NET"`

### 3. Custom Rule Syntax Error

Initial rule formatting caused Suricata to reject the rule.

Correct rule:

`alert icmp any any -> any any (msg:"ICMP_TEST"; sid:1000001; rev:1;)`

### 4. Wrong Capture Interface

Using:

`-i 8`

was not always interpreted by Suricata as the correct Windows capture device.

Solution:

Used:

`"C:\Program Files\Wireshark\tshark.exe" -D`

to identify the correct Npcap device.

### 5. No Alert in eve.json

Troubleshooting approach:

First verified traffic with TShark.

TShark confirmed:

`192.168.56.103 → 192.168.56.1`

Then Suricata was started on the same capture interface.

After generating ICMP traffic, the custom rule successfully generated alerts.

---

## 📚 Key Concepts Learned

### IDS

Intrusion Detection System.

Suricata observes network traffic and generates alerts when traffic matches detection rules.

### Npcap

Packet capture driver used by Windows applications such as Wireshark, TShark and Suricata.

### Detection Rule

Defines what type of traffic Suricata should detect.

### Signature ID (SID)

Unique identifier for a Suricata rule.

### fast.log

Simple human-readable alert log.

### eve.json

Structured JSON security event log that can later be consumed by SIEM platforms.

### TShark

Command-line version of Wireshark used to verify packet capture and troubleshoot network visibility.

---

## 🎯 Lab Result

Successfully demonstrated:

- ✅ Suricata 8.0.6 installation
- ✅ Suricata configuration
- ✅ Npcap packet capture
- ✅ Windows interface identification
- ✅ Custom ICMP detection rule
- ✅ Kali → Windows traffic generation
- ✅ Packet verification using TShark
- ✅ Suricata IDS detection
- ✅ Alert generation in `fast.log`
- ✅ Alert generation in `eve.json`
- ✅ Basic SOC troubleshooting methodology

---

## 🧠 Revision Cheat Sheet

Remember this:

TRAFFIC
↓
NPCAP
↓
SURICATA
↓
RULE
↓
MATCH
↓
ALERT
↓
FAST.LOG + EVE.JSON

Important files:

`suricata.yaml` → Configuration

`local.rules` → Detection rules

`fast.log` → Human-readable alerts

`eve.json` → Structured security events

Important IPs:

`Kali = 192.168.56.103`

`Windows = 192.168.56.1`

Test:

`ping 192.168.56.1`

Detection:

`ICMP_TEST`

---

## 🏁 Final Takeaway

This lab demonstrated the complete basic SOC detection pipeline:

**Generate Traffic → Capture Packets → Analyze Traffic → Match Detection Rule → Generate Alert → Investigate Logs**

This forms the foundation for later SOC labs involving:

**Port Scanning → Brute Force → SSH Attacks → Web Attacks → DNS Attacks → SIEM Detection → Incident Investigation**