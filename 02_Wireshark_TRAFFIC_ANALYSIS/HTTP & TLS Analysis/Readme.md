# 🔐  HTTPS AND TLS Handshake Investigation

## 📌 Objective

The objective of this lab was to understand how HTTPS communication works by analyzing the **TLS Handshake** in Wireshark.

Unlike HTTP, HTTPS encrypts all application data. As SOC Analysts, we cannot read the actual webpage contents, but we can still investigate the handshake process, identify the destination domain, TLS version, and determine whether a secure connection was successfully established.

---

# 🧠 Scenario

A user browses a website using HTTPS.

Our task is to capture the network traffic and investigate how the secure connection is established before any encrypted communication begins.

---

# 🛠️ Lab Setup

| Component | Details |
|-----------|----------|
| Attacker | N/A (Normal User Activity) |
| Monitoring Machine | Windows Host |
| Tool | Wireshark |
| Protocol | TLS 1.2 / TLS 1.3 |
| Browser | Microsoft Edge / Chrome |

---

# 📚 Concepts Learned

- Difference between HTTP and HTTPS
- Why HTTPS is encrypted
- TLS Handshake
- Client Hello
- Server Hello
- Change Cipher Spec
- Application Data
- Server Name Indication (SNI)
- Cipher Suites
- TLS Version Negotiation
- Why SOC Analysts can still investigate encrypted traffic

---

# 🔬 Methodology

## Step 1 — Start Packet Capture

Open Wireshark and start capturing packets on the active network interface.

---

## Step 2 — Generate HTTPS Traffic

Visit websites like

- https://example.com
- https://google.com

This generates HTTPS traffic that uses TLS.

---

## Step 3 — Apply TLS Filter

```
tls
```

This filter displays only TLS packets.

---

## Step 4 — Locate Client Hello

Look for packets similar to

```
Client Hello (SNI=example.com)
```

This is the first packet sent by the client to initiate a secure connection.

---

## Step 5 — Identify the Client

Observe the Source IP.

Example

```
Source:
2401:4900:....
```

The source IP belongs to the client (our Windows machine).

The client always sends the **Client Hello**.

---

## Step 6 — Identify the Server

The next packet is

```
Server Hello
```

The Source IP now belongs to the web server.

Example

```
2606:4700:...
```

The server responds by selecting the encryption parameters.

---

## Step 7 — Observe the TLS Handshake

Typical sequence observed:

```
Client Hello

↓

Server Hello

↓

Change Cipher Spec

↓

Application Data

↓

Encrypted Communication
```

This confirms the TLS handshake completed successfully.

---

## Step 8 — Investigate Client Hello

Expand

```
Transport Layer Security

↓

Handshake Protocol

↓

Client Hello
```

Important fields to inspect:

- Supported TLS Versions
- Cipher Suites
- Extensions
- Supported Groups
- Key Share
- Server Name Indication (SNI)

---

## Step 9 — Verify the Requested Domain

Locate

```
Server Name Indication (SNI)
```

Example

```
example.com
```

Although HTTPS encrypts the webpage contents, the SNI remains visible and helps analysts identify the destination domain.

---

## Step 10 — Observe Encrypted Traffic

After the handshake, packets appear as

```
Application Data

Application Data

Application Data
```

This indicates that encryption is active.

The packet contents cannot be viewed without the session keys.

---

# 🔍 Investigation Questions

### Who initiated the connection?

The Windows host sent the **Client Hello**, making it the client.

---

### Who responded?

The remote web server replied with the **Server Hello**.

---

### Which domain was accessed?

Identified through the SNI field.

Example

```
example.com
```

---

### Which protocol secured the connection?

TLS 1.3 (or TLS 1.2 depending on the session).

---

### Can we see HTTP requests?

No.

HTTPS encrypts:

- URLs (except domain via SNI)
- Cookies
- Credentials
- HTML
- API Requests
- Response Data

Only metadata remains visible.

---

# 📊 HTTP vs HTTPS Comparison

| HTTP | HTTPS |
|-------|---------|
| Data visible | Data encrypted |
| GET request visible | Hidden |
| Host visible | Hidden after handshake (except SNI) |
| Credentials visible | Encrypted |
| Cookies visible | Encrypted |
| Content readable | Not readable |

---

# 🔑 TLS Handshake Flow

```
Browser

↓

Client Hello

↓

Server Hello

↓

Cipher Suite Selected

↓

Encryption Keys Exchanged

↓

Secure Session Created

↓

Encrypted Application Data
```

---

# 🧠 SOC Analyst Takeaways

During a TLS investigation, a SOC Analyst should verify:

- Who initiated the connection
- Destination server
- Requested domain (SNI)
- TLS version
- Cipher suite negotiation
- Successful handshake
- Transition to encrypted communication

Even without decrypting HTTPS traffic, these artifacts provide valuable evidence during incident investigations.

---

# ✅ Skills Practiced

- Packet Capture
- TLS Filtering
- HTTPS Traffic Analysis
- Client Hello Investigation
- Server Hello Investigation
- SNI Analysis
- TLS Handshake Analysis
- Network Traffic Investigation
- Wireshark Navigation
- SOC Investigation Methodology

---

# 📖 Key Learning

HTTPS traffic cannot be inspected like HTTP traffic because the application data is encrypted.

However, SOC Analysts can still analyze:

- Connection initiation
- Destination domains
- TLS versions
- Cipher negotiation
- Handshake success
- Communication patterns

This metadata is often sufficient to detect suspicious encrypted communications and investigate potential security incidents.

---

## 🏁 Conclusion

This lab demonstrated how secure HTTPS communication is established using the TLS handshake. By analyzing Client Hello, Server Hello, SNI, and encrypted Application Data, we learned how security analysts investigate encrypted network traffic without needing to decrypt the actual content.

This is a fundamental skill for Blue Team operations, SOC monitoring, threat hunting, and network forensic investigations.