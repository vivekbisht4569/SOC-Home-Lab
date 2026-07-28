# 📂 Lab 07 – FTP Traffic Analysis with Wireshark

> **Category:** Network Traffic Analysis  
> **Difficulty:** Beginner → Intermediate  
> **Tools Used:** Wireshark, Kali Linux, Windows CMD, vsFTPd, VirtualBox  
> **Protocol:** FTP (File Transfer Protocol)  
> **Focus:** FTP Authentication, Plaintext Credentials, TCP Port 21, Active FTP, Directory Listing Analysis

---

# 🎯 Objective

The objective of this lab was to understand how the **FTP protocol works**, investigate an FTP session using **Wireshark**, and observe why FTP is considered an **insecure protocol**.

Unlike HTTPS or SSH, FTP transmits authentication information in **plain text**, allowing anyone monitoring the network to capture usernames and passwords.

---

# 🖥️ Lab Environment

```
                  Home Lab

        +------------------------+
        | Windows Host           |
        | FTP Client             |
        | Wireshark              |
        +-----------+------------+
                    |
            FTP Traffic (TCP/21)
                    |
        +-----------+------------+
        | Kali Linux VM          |
        | vsFTPd FTP Server      |
        +------------------------+
```

---

# 🛠 Tools Used

- Kali Linux
- Windows 11
- VirtualBox
- Wireshark
- vsFTPd FTP Server
- Windows FTP Client

---

# 📚 Concepts Learned

- FTP Control Channel
- FTP Authentication
- TCP Three-Way Handshake
- FTP Banner
- FTP Commands
- Active FTP (PORT Command)
- Directory Listing
- Plaintext Credential Exposure
- FTP Responses
- Network Traffic Investigation

---

# ⚙️ Lab Setup

## Step 1 – Install FTP Server

```bash
sudo apt update
sudo apt install vsftpd -y
```

---

## Step 2 – Start FTP Service

```bash
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

Verify

```bash
sudo systemctl status vsftpd
```

---

## Step 3 – Verify Port 21

```bash
sudo ss -tulnp | grep :21
```

Expected

```
LISTEN 0 32 *:21
```

---

## Step 4 – Connect from Windows

```cmd
ftp 192.168.56.102
```

Login using Kali credentials.

---

## Step 5 – Generate FTP Traffic

Commands executed

```cmd
pwd
ls
dir
cd Desktop
pwd
bye
```

These commands generated FTP packets for investigation.

---

# 🔍 Investigation Methodology

## Phase 1 — Identify FTP Traffic

Apply filter

```
ftp
```

Questions answered

- Is FTP being used?
- Which IP initiated the connection?
- Which IP is the FTP server?

---

## Phase 2 — TCP Handshake

Filter

```
tcp.port == 21
```

Observe

```
SYN
↓

SYN, ACK
↓

ACK
```

Determine

- Client IP
- Server IP
- Destination Port

---

## Phase 3 — Authentication Investigation

Inspect FTP packets.

Identify

```
USER kali
PASS kali
```

Verify server response

```
230 Login successful
```

---

## Phase 4 — FTP Commands

Observe client commands

```
XPWD
NLST
PORT
QUIT
```

Understand their purpose.

---

## Phase 5 — Server Responses

Observed

```
220
331
230
150
226
221
```

Each response indicates a different stage of the FTP session.

---

## Phase 6 — Security Analysis

Evaluate whether credentials are encrypted.

Determine protocol security.

---

# 🔬 Investigation Findings

## Client Information

| Field | Value |
|--------|-------|
| Client IP | 192.168.56.1 |
| Server IP | 192.168.56.102 |
| Protocol | FTP |
| Destination Port | TCP 21 |

---

## TCP Handshake

Observed

```
SYN
↓

SYN ACK

↓

ACK
```

Connection established successfully.

---

## FTP Banner

```
220 (vsFTPd 3.0.5)
```

This banner reveals

- FTP Server Software
- Software Version
- Service Availability

---

## Authentication

Username observed

```
USER kali
```

Password observed

```
PASS kali
```

Server Response

```
230 Login successful
```

Authentication completed successfully.

---

# 🚨 Critical Security Finding

One of the most important observations during this investigation was that the FTP protocol transmitted the user's credentials **without any encryption**.

The captured packets clearly revealed:

```
USER kali

PASS kali
```

This demonstrates that **FTP sends usernames and passwords in plaintext**.

Any attacker performing packet sniffing on the same network could immediately capture these credentials and gain unauthorized access.

This is one of the primary reasons why FTP is considered an insecure protocol for modern environments.

> **SOC Analyst Observation:** Credentials can be extracted directly from captured packets without any decryption.
> 
---

# 📂 FTP Commands Observed

| Command | Purpose |
|----------|----------|
| USER | Sends username |
| PASS | Sends password |
| XPWD | Display current directory |
| NLST | Directory listing |
| PORT | Active FTP data connection |
| QUIT | Close session |

---

# 📨 FTP Server Responses

| Code | Meaning |
|------|----------|
| 220 | FTP Server Ready |
| 331 | Password Required |
| 230 | Login Successful |
| 150 | Preparing Directory Listing |
| 226 | Transfer Completed |
| 221 | Connection Closed |

---

# 📸 Screenshots

## TCP Handshake

```
images/tcp_handshake.png
```

---

## FTP Authentication

```
images/ftp_login.png
```

---

## Plaintext Password

```
images/ftp_plaintext_password.png
```

---

## FTP Commands

```
images/ftp_commands.png
```

---

## FTP Responses

```
images/ftp_responses.png
```

---

# 🧠 Key Learning Outcomes

✔ Understood how FTP establishes a connection.

✔ Investigated the TCP Three-Way Handshake.

✔ Learned the purpose of the FTP banner.

✔ Identified FTP authentication packets.

✔ Observed username and password transmission.

✔ Investigated FTP commands executed by the client.

✔ Understood FTP response codes.

✔ Learned how Active FTP creates data connections.

✔ Performed a complete FTP session investigation using Wireshark.

✔ Understood why FTP should never be used for transmitting sensitive credentials.

---

# 🛡️ Security Recommendation

FTP provides **no encryption** for authentication or data transfer.

For secure file transfer, organizations should replace FTP with:

- SFTP (SSH File Transfer Protocol)
- FTPS (FTP over TLS/SSL)

These protocols encrypt credentials and transferred files, protecting them from packet sniffing attacks.

---

# 📖 SOC Analyst Takeaway

During this lab, I investigated an FTP session from start to finish using Wireshark. I successfully identified the TCP handshake, server banner, authentication process, FTP commands, server responses, and one of the most critical security weaknesses of FTP—**plaintext transmission of usernames and passwords**.

This exercise demonstrates how a SOC Analyst can reconstruct user activity from network traffic and identify insecure protocols that expose sensitive information.