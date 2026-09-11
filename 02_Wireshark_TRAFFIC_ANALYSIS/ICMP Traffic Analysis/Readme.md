# 🌐ICMP (Ping) Investigation

## 📌 Objective

The objective of this lab was to understand how **ICMP (Internet Control Message Protocol)** works by capturing and analyzing **Echo Request** and **Echo Reply** packets using Wireshark.

The focus of this lab was not only to understand the ICMP protocol but also to learn how a **SOC Analyst investigates host reachability and reconnaissance activity**.

---

# 🛠️ Lab Environment

| Component | Details |
|-----------|----------|
| Attacker | Kali Linux VM |
| Victim | Windows Host |
| Monitoring Tool | Wireshark |
| Protocol | ICMP |

---

# 🎯 Lab Goals

- Understand ICMP communication
- Capture ICMP packets
- Identify Echo Requests and Echo Replies
- Investigate host reachability
- Learn the difference between normal ping activity and reconnaissance
- Follow a structured SOC investigation methodology

---

# 🌍 Real World Scenario

A SOC Analyst receives an alert that one host is repeatedly sending ICMP packets to another host.

The analyst must answer:

- Who initiated the communication?
- Which host responded?
- Was the destination reachable?
- Is this normal activity or reconnaissance?

---

# 📚 What is ICMP?

ICMP (Internet Control Message Protocol) is a Layer 3 protocol used for:

- Network diagnostics
- Error reporting
- Connectivity testing
- Host discovery

Unlike TCP and UDP, ICMP does **not** establish connections or transfer application data.

---

# 🔬 Lab Steps

## Step 1 – Start Packet Capture

Open Wireshark.

Select the active network interface.

Start packet capture.

---

## Step 2 – Generate ICMP Traffic

From Kali Linux run:

```bash
ping <Windows-IP>
```

Example

```bash
ping 192.168.56.1
```

Allow several packets to be exchanged.

Stop the ping using:

```bash
Ctrl + C
```

---

## Step 3 – Stop Packet Capture

Return to Wireshark and stop the capture.

---

## Step 4 – Filter ICMP Packets

Apply the display filter:

```wireshark
icmp
```

Only ICMP packets are displayed.

---

## Step 5 – Identify Echo Requests

Filter:

```wireshark
icmp.type == 8
```

These packets are sent by the client.

Example:

```
Kali

↓

Echo Request

↓

Windows
```

---

## Step 6 – Identify Echo Replies

Filter:

```wireshark
icmp.type == 0
```

These packets are sent by the destination host.

Example:

```
Windows

↓

Echo Reply

↓

Kali
```

---

## Step 7 – Inspect ICMP Header

Expand

```
Internet Control Message Protocol
```

Observe:

- Type
- Code
- Checksum
- Identifier
- Sequence Number

---

## Step 8 – Correlate Request and Reply

Verify that:

- Identifier matches
- Sequence Number matches

This confirms the Echo Reply belongs to the corresponding Echo Request.

---

## Step 9 – Build Timeline

Observe the packet sequence:

```
Echo Request

↓

Echo Reply

↓

Echo Request

↓

Echo Reply

↓

Echo Request

↓

Echo Reply
```

Successful communication indicates the host is reachable.

---

#  SOC Investigation Methodology

Always investigate ICMP traffic using the following workflow.

```
Traffic Captured

↓

Apply ICMP Filter

↓

Identify Source Host

↓

Identify Destination Host

↓

Separate Requests and Replies

↓

Inspect ICMP Header

↓

Match Request ↔ Reply

↓

Count Packets

↓

Measure Response Time

↓

Determine Intent

↓

Write Investigation Findings
```

---

# 📊 Evidence to Collect

During every ICMP investigation collect:

- Source IP
- Destination IP
- Echo Request Count
- Echo Reply Count
- ICMP Type
- Identifier
- Sequence Number
- Packet Timing
- Packet Loss

---

#  ICMP Packet Types

## Echo Request

```
Type: 8
```

Purpose

```
"Are you reachable?"
```

---

## Echo Reply

```
Type: 0
```

Purpose

```
"Yes, I am reachable."
```

---

# 📖 Wireshark Filters Used

Show all ICMP traffic

```wireshark
icmp
```

---

Echo Requests

```wireshark
icmp.type == 8
```

---

Echo Replies

```wireshark
icmp.type == 0
```

---

Specific Host

```wireshark
ip.addr == <Kali-IP>
```

---

#  Investigation Questions

During every ICMP investigation, answer:

- Who initiated the communication?
- Which host responded?
- Was the host reachable?
- How many Echo Requests were sent?
- How many Echo Replies were received?
- Was there packet loss?
- Was this normal traffic or reconnaissance?

---

# 🌐 How ICMP Works

```
Kali Linux

↓

Echo Request

↓

Windows Host

↓

Echo Reply

↓

Kali Linux
```

If no Echo Reply is received:

```
Host Unreachable

or

Firewall Blocking ICMP
```

---

# SOC Relevance

SOC Analysts investigate ICMP traffic to identify:

- Host Discovery
- Ping Sweeps
- Internal Reconnaissance
- Malware Beaconing
- Network Reachability Issues

Repeated ICMP requests to multiple hosts may indicate the reconnaissance phase of an attack.

---

#  Skills Learned

- ICMP Packet Analysis
- Echo Request Investigation
- Echo Reply Investigation
- Host Reachability Analysis
- Packet Correlation
- Network Timeline Analysis
- Wireshark Filtering
- SOC Investigation Methodology

---

#  Conclusion

In this lab, ICMP traffic was generated from a Kali Linux VM to a Windows host and captured using Wireshark.

The investigation successfully identified:

- The source host
- The destination host
- Echo Requests
- Echo Replies
- Packet sequence
- Host reachability

This lab demonstrated how SOC Analysts investigate ICMP communication to determine network connectivity and identify potential reconnaissance activity before an attacker proceeds to later stages of an attack.