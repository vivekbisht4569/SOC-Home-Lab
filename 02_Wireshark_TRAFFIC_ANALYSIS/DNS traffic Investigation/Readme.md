# 🌐 DNS Traffic Analysis & DNS Record Investigation

## 📌 Overview

In this lab, I learned how the Domain Name System (DNS) works by capturing and analyzing live DNS packets using **Wireshark**.

The objective was to understand how a hostname is resolved, how reverse DNS lookups work, and how different DNS record types appear in real network traffic.

This lab also strengthened my ability to investigate DNS traffic from a SOC analyst's perspective.

---

# 🎯 Lab Objectives

- Understand how DNS works
- Capture DNS traffic using Wireshark
- Analyze DNS Query and DNS Response packets
- Learn the purpose of different DNS Record Types
- Perform Reverse DNS Lookup (PTR Record)
- Correlate DNS traffic with previous HTTP investigations
- Build a DNS investigation methodology used by SOC Analysts

---

# 🖥️ Lab Environment

| Component | Details |
|-----------|----------|
| Host Machine | Windows 11 |
| Tool Used | Wireshark |
| Network | Home WiFi |
| Traffic Generated | Visiting websites in browser |

---

# 🧠 DNS Investigation Methodology

Whenever investigating DNS traffic, I follow this workflow.

```
1. Apply DNS Filter

↓

2. Find DNS Query

↓

3. Identify Query Type

↓

4. Identify Requested Domain/IP

↓

5. Open DNS Response

↓

6. Check Returned Answer

↓

7. Identify Destination IP or Domain

↓

8. Correlate with HTTP/TCP Traffic

↓

9. Determine if Activity is Legitimate or Suspicious
```

---

# 🔍 Step 1 – Capture DNS Traffic

Started packet capture in Wireshark.

Applied filter

```
dns
```

Only DNS packets became visible.

---

# 🔍 Step 2 – Identify DNS Query

Observed packets similar to

```
Standard Query
```

Example

```
Standard query PTR
45.124.223.34.in-addr.arpa
```

---

# 🔍 Step 3 – Identify Query Type

Observed

```
PTR
```

Meaning

```
Reverse DNS Lookup
```

Instead of

```
Domain → IP
```

Windows was asking

```
IP → Domain
```

---

# 🔍 Step 4 – Analyze DNS Response

DNS server replied

```
ec2-34-223-124-45.us-west-2.compute.amazonaws.com
```

This revealed that

```
34.223.124.45

↓

Amazon EC2

↓

AWS Cloud Server
```

---

# 🔍 Step 5 – Correlation with Previous HTTP Investigation

During the previous HTTP lab, I had already discovered

```
Destination IP

34.223.124.45
```

DNS investigation confirmed that the same IP belongs to

```
ec2-34-223-124-45.us-west-2.compute.amazonaws.com
```

This demonstrated how DNS resolution happens before HTTP communication.

---

# 📊 Investigation Summary

### Query Type

```
PTR
```

### Reverse Lookup

```
45.124.223.34.in-addr.arpa
```

### DNS Response

```
ec2-34-223-124-45.us-west-2.compute.amazonaws.com
```

### Destination

```
Amazon EC2 (AWS)
```

### Conclusion

The Windows machine successfully performed a reverse DNS lookup to identify the hostname associated with the destination IP. The DNS response matched the IP observed during the previous HTTP investigation, confirming normal DNS resolution before web communication.

---

# 🌐 How DNS Works

```
User opens website

↓

Browser needs IP Address

↓

DNS Query sent

↓

DNS Server responds

↓

Browser receives IP Address

↓

TCP Connection

↓

HTTP / HTTPS Request

↓

Website Loads
```

---

# 🛡️ SOC Investigation Process

Instead of simply reading packets, I learned how to reconstruct an entire network conversation.

```
DNS Query

↓

DNS Response

↓

Resolved IP

↓

TCP Connection

↓

HTTP Request

↓

HTTP Response
```

This approach is commonly used by SOC Analysts during network investigations.

---

# 📚 DNS Record Cheat Sheet

| Record | Purpose | Example |
|---------|---------|---------|
| **A** | Domain → IPv4 Address | google.com → 142.250.x.x |
| **AAAA** | Domain → IPv6 Address | google.com → 2404:6800::xxxx |
| **PTR** | IP → Domain (Reverse Lookup) | 34.223.124.45 → ec2-34-223-124-45.us-west-2.compute.amazonaws.com |
| **CNAME** | Alias of another domain | www.example.com → example.com |
| **MX** | Mail Server Record | gmail.com → gmail-smtp-in.l.google.com |
| **NS** | Name Server | ns1.google.com |
| **TXT** | Text Records (SPF, DKIM, DMARC) | Email authentication |
| **SOA** | Start of Authority | DNS Zone Information |
| **SRV** | Service Location | LDAP, Kerberos, SIP |
| **CAA** | Certificate Authority Authorization | Controls SSL Certificate Issuers |

---

# ⭐ Priority DNS Records for SOC Analysts

### ⭐⭐⭐⭐⭐ Must Know

- A
- AAAA
- PTR
- CNAME

---

### ⭐⭐⭐⭐ Common

- MX
- TXT
- NS

---

### ⭐⭐⭐ Advanced

- SOA
- SRV
- CAA

---

# 🧠 Key Learnings

- Understood how DNS resolves domain names.
- Learned the difference between A, AAAA, PTR, and CNAME records.
- Performed reverse DNS lookups using PTR records.
- Correlated DNS traffic with previous HTTP investigations.
- Learned how DNS fits into the complete network communication process.
- Built a repeatable SOC investigation methodology for DNS traffic analysis.

---

# ✅ Skills Practiced

- Wireshark Packet Analysis
- DNS Investigation
- Reverse DNS Lookup
- DNS Record Identification
- Network Traffic Correlation
- HTTP & DNS Correlation
- SOC Investigation Methodology
- Threat Hunting Fundamentals

---

