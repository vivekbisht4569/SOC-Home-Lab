# 📂 Lab 08 – SSH Traffic Analysis with Wireshark

> **Category:** Network Traffic Analysis  
> **Difficulty:** Beginner → Intermediate  
> **Tools Used:** Wireshark, Kali Linux, Windows PowerShell, OpenSSH Server, VirtualBox  
> **Protocol:** SSH (Secure Shell)  
> **Focus:** Secure Remote Login, SSH Handshake, Key Exchange, Encryption Analysis

---

# 🎯 Objective

The objective of this lab was to understand how **SSH (Secure Shell)** establishes a secure remote connection between a client and a server.

Unlike the previous FTP lab, this investigation focuses on **encrypted communication** and demonstrates why SSH is the preferred protocol for remote administration.

---

# 🖥️ Lab Environment

```
                  Home Lab

      +---------------------------+
      | Windows Host              |
      | SSH Client                |
      | Wireshark                 |
      +-------------+-------------+
                    |
             SSH (TCP Port 22)
                    |
      +-------------+-------------+
      | Kali Linux VM             |
      | OpenSSH Server            |
      +---------------------------+
```

---

# 🛠 Tools Used

- Kali Linux
- Windows 11
- OpenSSH Server
- Windows OpenSSH Client
- Wireshark
- VirtualBox

---

# 📚 Concepts Learned

- SSH Protocol
- TCP Three-Way Handshake
- SSH Banner Exchange
- SSH Version Negotiation
- Key Exchange
- Elliptic Curve Diffie-Hellman (ECDH)
- Session Encryption
- Secure Authentication
- Difference between FTP and SSH

---

# ⚙️ Lab Setup

## Step 1 – Verify SSH Server

```bash
sudo systemctl status ssh
```

Verified that the SSH service was running.

---

## Step 2 – Verify Port 22

```bash
sudo ss -tulnp | grep :22
```

Confirmed that SSH was listening on TCP Port 22.

---

## Step 3 – Verify Kali IP

```bash
hostname -I
```

Kali IP:

```
192.168.56.102
```

---

## Step 4 – Start Wireshark

Started packet capture on the Host-Only network adapter before initiating the SSH connection.

---

## Step 5 – Connect from Windows

Using PowerShell:

```powershell
ssh kali@192.168.56.102
```

Accepted the SSH fingerprint on first connection and authenticated using the Kali user's password.

---

## Step 6 – Generate SSH Traffic

Executed the following commands after login:

```bash
pwd

whoami

hostname

ls

id

exit
```

These commands generated encrypted SSH traffic for analysis.

---

# 🔍 Investigation Methodology

## Phase 1 — Identify SSH Traffic

Applied filter:

```
ssh
```

Observed all SSH packets exchanged between the Windows client and the Kali server.

---

## Phase 2 — TCP Handshake

Confirmed the TCP connection using:

```
SYN

↓

SYN, ACK

↓

ACK
```

This established the TCP session before SSH communication began.

---

## Phase 3 — SSH Banner Exchange

Observed:

```
Client:
SSH-2.0-OpenSSH_for_Windows_9.5

Server:
SSH-2.0-OpenSSH_10.2p1 Debian-5
```

This identifies the SSH software and version running on both systems.

---

## Phase 4 — Key Exchange

Observed packets:

```
Key Exchange Init

↓

Elliptic Curve Diffie-Hellman Key Exchange Init

↓

Elliptic Curve Diffie-Hellman Reply
```

During this phase, the client and server negotiated encryption algorithms and securely established a shared session key.

---

## Phase 5 — Session Encryption

Observed:

```
New Keys
```

After this packet, the session switched to encrypted communication.

---

## Phase 6 — Encrypted Communication

All subsequent packets appeared as:

```
Encrypted Packet
```

No usernames, passwords, commands, or command output were visible.

---

# 🔬 Investigation Findings

## Client Information

| Field | Value |
|--------|-------|
| Client IP | 192.168.56.1 |
| Server IP | 192.168.56.102 |
| Protocol | SSH |
| Port | TCP 22 |

---

## SSH Version Exchange

### Client

```
SSH-2.0-OpenSSH_for_Windows_9.5
```

### Server

```
SSH-2.0-OpenSSH_10.2p1 Debian-5
```

This banner exchange identifies the SSH implementation and software version used by both endpoints.

---

## Key Exchange

Observed:

```
Key Exchange Init

Elliptic Curve Diffie-Hellman Key Exchange

New Keys
```

This process securely establishes encryption before authentication.

---

# 🔐 Security Analysis

Unlike FTP, the SSH session encrypted all authentication and user activity.

The investigation confirmed that:

❌ Username was **not visible**

❌ Password was **not visible**

❌ Commands executed were **not visible**

❌ Command output was **not visible**

Instead, Wireshark displayed:

```
Encrypted Packet
```

for all application data after the key exchange.

---

# 🚨 Critical Security Finding

The most important observation during this investigation was that **SSH encrypts the entire communication session** after the key exchange.

Although packets were successfully captured, sensitive information such as usernames, passwords, and executed commands could not be viewed.

This demonstrates why SSH is considered a secure protocol for remote administration.

> **SOC Analyst Observation:** Network traffic can confirm that an SSH session occurred, but encrypted payloads prevent attackers from reading credentials or command data.

📷 **Screenshot Placeholder:**  
`images/ssh_encrypted_packets.png`

---

# 📂 SSH Packets Observed

| Packet | Purpose |
|---------|----------|
| SSH Banner | Version Exchange |
| Key Exchange Init | Encryption Negotiation |
| Elliptic Curve Diffie-Hellman | Secure Key Exchange |
| New Keys | Encryption Enabled |
| Encrypted Packet | Secure Communication |

---

# 📊 FTP vs SSH Comparison

| Feature | FTP | SSH |
|----------|-----|-----|
| Default Port | 21 | 22 |
| Username Visible | ✅ Yes | ❌ No |
| Password Visible | ✅ Yes | ❌ No |
| Commands Visible | ✅ Yes | ❌ No |
| Encryption | ❌ No | ✅ Yes |
| Secure Remote Access | ❌ No | ✅ Yes |

---

# 📸 Screenshots

## SSH Connection

```
images/ssh_login.png
```

---

## TCP Handshake

```
images/ssh_handshake.png
```

---

## SSH Banner Exchange

```
images/ssh_banner.png
```

---

## Key Exchange

```
images/ssh_key_exchange.png
```

---

## Encrypted Packets

```
images/ssh_encrypted_packets.png
```

---

# 🧠 Key Learning Outcomes

✔ Understood how SSH establishes a secure connection.

✔ Identified the TCP Three-Way Handshake.

✔ Observed SSH version negotiation (banner exchange).

✔ Learned how Key Exchange works.

✔ Understood the role of Elliptic Curve Diffie-Hellman in establishing secure communication.

✔ Observed the transition to encrypted communication using **New Keys**.

✔ Verified that usernames, passwords, and commands are encrypted.

✔ Compared SSH security with FTP.

✔ Performed a complete SSH traffic investigation using Wireshark.

---

# 🛡️ Security Recommendation

SSH should always be preferred over FTP, Telnet, or other plaintext remote access protocols.

Organizations should:

- Use SSH for remote administration.
- Disable insecure services such as Telnet and FTP whenever possible.
- Keep OpenSSH updated to the latest version.
- Use strong passwords or SSH key authentication for better security.

---

# 📖 SOC Analyst Takeaway

During this lab, I investigated a complete SSH session using Wireshark. I identified the TCP handshake, SSH version exchange, key exchange process, and the point where encrypted communication began.

Unlike the FTP investigation, I confirmed that SSH successfully protects sensitive information by encrypting the entire session, preventing usernames, passwords, commands, and output from being exposed in captured network traffic.

This lab strengthened my understanding of secure remote access protocols and how SOC analysts investigate encrypted network sessions while recognizing the limits of packet inspection after encryption is established.