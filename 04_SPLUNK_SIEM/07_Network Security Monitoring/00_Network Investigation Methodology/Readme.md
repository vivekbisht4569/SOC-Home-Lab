#  Network Investigation Methodology

##  Overview

This document describes the investigation methodology followed throughout my Network Security Monitoring (NSM) labs.

The goal is **not** to memorize Wireshark filters or protocol fields.

The goal is to think like a SOC analyst by asking the right questions, collecting evidence, analyzing communication, and reaching a conclusion.

---

# 🧠 Core SOC Mindset

A beginner starts with:

```
Which Wireshark filter should I use?
```

A SOC analyst starts with:

```
What happened?

Who communicated?

How did they communicate?

What evidence supports it?

Is the activity normal or suspicious?
```

---

# 🔍 Network Investigation Workflow

Every network investigation follows this workflow.

```text
Alert / Packet Capture

          ↓

1. Understand the Activity

          ↓

2. Identify Endpoints

          ↓

3. Identify Protocol

          ↓

4. Build Communication Timeline

          ↓

5. Analyze Packet Behavior

          ↓

6. Follow the Conversation

          ↓

7. Collect Evidence

          ↓

8. Determine Verdict
```

---

# Step 1 — Understand the Activity

## Question

What kind of activity am I investigating?

Examples:

| Activity | Investigation Type |
|----------|--------------------|
| Ping | ICMP |
| Website Visit | HTTP / HTTPS |
| Domain Lookup | DNS |
| Port Scan | TCP |
| File Sharing | SMB |
| Remote Login | SSH / RDP |

---

# Step 2 — Identify Endpoints

## Question

Who communicated?

Every packet has two endpoints.

Identify:

- Source IP
- Destination IP

Navigation:

```
Statistics

      ↓

Conversations
```

Example:

```
192.168.1.10

        ↓

8.8.8.8
```

Meaning:

Local machine communicated with Google's DNS server.

---

# Step 3 — Identify the Protocol

## Question

How did they communicate?

Determine the protocol being used.

Common protocols:

| Protocol | Purpose |
|----------|----------|
| DNS | Name Resolution |
| TCP | Reliable Communication |
| UDP | Fast Communication |
| ICMP | Network Connectivity |
| HTTP | Unencrypted Web Traffic |
| HTTPS / TLS | Encrypted Web Traffic |
| SMB | File Sharing |
| SSH | Secure Remote Access |

---

# Step 4 — Build a Timeline

## Question

When did the communication happen?

Use timestamps to reconstruct events.

Example:

```
10:00:01

DNS Query

        ↓

10:00:02

TCP Connection

        ↓

10:00:03

TLS Session

        ↓

10:00:04

HTTPS Data Transfer
```

A timeline explains the sequence of events instead of looking at packets individually.

---

# Step 5 — Analyze Packet Behavior

## Question

What exactly happened?

Look inside the packet.

Examples:

DNS

- Requested Domain
- Returned IP Address

TCP

- SYN
- SYN-ACK
- ACK

HTTP

- GET
- POST
- URI
- User-Agent

ICMP

- Echo Request
- Echo Reply

---

# Step 6 — Follow the Conversation

## Question

Can I reconstruct the communication?

Navigation:

```
Right Click Packet

        ↓

Follow

        ↓

TCP Stream
```

This rebuilds the communication between client and server.

Instead of isolated packets, you now see the complete conversation.

---

# Step 7 — Collect Evidence

Record important evidence.

Example:

```
Source IP

Destination IP

Protocol

Port

Timestamp

Domain

Packet Number

Observations
```

Evidence should always support your conclusion.

---

# Step 8 — Analyst Verdict

Every investigation ends with a conclusion.

Template:

```
Incident Type

Source IP

Destination IP

Protocol

Evidence

Findings

Verdict
```

Possible verdicts:

```
Benign

Suspicious

Requires Further Investigation
```

---

# SOC Investigation Questions

Every network investigation should answer these questions.

```
WHO?

(Source IP)

        ↓

TALKED TO WHO?

(Destination IP)

        ↓

HOW?

(Protocol + Port)

        ↓

WHAT HAPPENED?

(Packet Details)

        ↓

WHEN?

(Timeline)

        ↓

WHY?

(Context)

        ↓

BENIGN OR SUSPICIOUS?
```

---

# Splunk vs Network Investigation

| SIEM Investigation | Network Investigation |
|--------------------|----------------------|
| User | Source IP |
| Host | Endpoint |
| Event ID | Protocol |
| Process | Network Connection |
| Command Line | Packet Payload |
| Parent Process | Previous Packet |
| Timeline | Packet Timeline |

Although the evidence is different, the investigation mindset remains the same.

---

# Investigation Formula

Remember this process for every network investigation.

```text
Question

        ↓

Evidence Needed

        ↓

Identify Endpoints

        ↓

Identify Protocol

        ↓

Analyze Communication

        ↓

Build Timeline

        ↓

Collect Evidence

        ↓

Conclusion
```

---

# Key Lesson

Packets are evidence.

Wireshark is a packet analysis tool.

Protocols explain communication.

The real SOC skill is not knowing every protocol or every Wireshark filter.

The real SOC skill is knowing:

- What questions to ask
- What evidence is required
- How to interpret network communication
- How to determine whether activity is normal or suspicious

---

