# DNS & HTTP Traffic Analysis

## Objective

Learn how DNS and HTTP traffic appears in packet captures and understand how SOC analysts investigate domain requests and web activity.

---

## Lab Environment

| Machine | Role |
|----------|----------|
| Windows 11 | Client |
| Wireshark | Packet Analysis |
| Browser | HTTP Traffic |
| DNS Service | Name Resolution |

---

# Part 1 — DNS Analysis

## Traffic Generated

Command:

```bash
nslookup google.com
```

---

## Investigation

Wireshark Filter:

```text
dns
```

Observed:

```text
Standard Query:
google.com

Standard Query Response:
google.com → IP Address
```

Additional Observation:

```text
AAAA Query
AAAA Response
```

(IPv6 Address Resolution)

---

## DNS Analysis

DNS translates:

```text
google.com
↓
IP Address
```

Observed:

- DNS Query
- DNS Response
- IPv4 Resolution
- IPv6 Resolution

---

## SOC Relevance

DNS logs reveal:

- Domains visited
- External infrastructure contacted
- Potential malicious domains
- Malware command-and-control communications

DNS is often one of the first places analysts investigate during an incident.

---

# Part 2 — HTTP Analysis

## Traffic Generated

Website Visited:

```text
http://httpforever.com
```

---

## Investigation

Wireshark Filter:

```text
http
```

Observed:

```text
GET / HTTP/1.1

GET /js/init.min.js

GET /css/style.min.css

GET /images/banner.svg

HTTP/1.1 200 OK
```

---

## HTTP Analysis

Browser requested:

- HTML Page
- JavaScript Files
- CSS Files
- Images

Server responded:

```text
HTTP/1.1 200 OK
```

Meaning:

Request Successfully Processed.

---

## TCP Out-Of-Order Observation

Observed:

```text
[TCP Out-Of-Order]
```

Meaning:

A TCP segment arrived in a different order than expected.

Example:

```text
Expected:
Packet 1
Packet 2
Packet 3

Received:
Packet 1
Packet 3
Packet 2
```

Wireshark flags Packet 3 as:

```text
TCP Out-Of-Order
```

---

## Why This Happens

Common Reasons:

- Network latency
- Multiple simultaneous downloads
- Packet capture timing
- Browser loading many resources at once

During this lab, the browser was downloading:

- CSS files
- JavaScript files
- Images

which generated normal Out-Of-Order traffic.

---

## SOC Analyst Perspective

A single TCP Out-Of-Order packet is usually normal.

Further investigation is only required when combined with:

- High packet loss
- Excessive retransmissions
- Connection failures
- Network instability

---

## Skills Learned

- DNS Analysis
- Domain Resolution
- HTTP Request Analysis
- HTTP Response Analysis
- GET Requests
- Status Code Analysis
- TCP Behavior Analysis
- Web Traffic Investigation

---

## SOC Relevance

SOC analysts investigate DNS and HTTP traffic to identify:

- Suspicious domains
- Malware communications
- Phishing activity
- Command-and-Control traffic
- Web-based attacks

---

## Outcome

Successfully captured and analyzed DNS and HTTP traffic using Wireshark while learning how web communication appears at the packet level.