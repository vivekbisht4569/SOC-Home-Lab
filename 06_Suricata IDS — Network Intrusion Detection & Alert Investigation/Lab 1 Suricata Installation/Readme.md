# Lab 01 — Suricata Installation & Initial Setup

## 📌 Objective

Install and configure the basic Suricata environment on the **Windows host** and prepare it for network traffic monitoring from the **Kali Linux VM**.

## 🧪 Lab Environment

* **Host OS:** Windows
* **Attacker:** Kali Linux VM
* **Virtualization:** VirtualBox
* **Network:** VirtualBox Host-Only Network
* **Windows IP:** `192.168.56.1`
* **Kali IP:** `192.168.56.102`
* **IDS:** Suricata
* **Packet Capture:** Npcap

## 🔧 Installation

### 1. Install Npcap

Npcap is required by Suricata on Windows for live packet capture.

Verified that Npcap was installed correctly and that the required packet-capture libraries were available.

### 2. Install Suricata

Installed Suricata on the Windows host.

Installation directory:

```text
C:\Program Files\Suricata
```

### 3. Verify Installation

Verified Suricata using:

```cmd
suricata.exe --build-info
```

Suricata successfully executed after installing Npcap.

## 🌐 Lab Network

```text
Kali Linux VM
192.168.56.102
      │
      │ VirtualBox Host-Only Network
      │
      ▼
Windows Host
192.168.56.1
      │
      ▼
Suricata IDS
```

## 🧠 What I Learned

* Suricata can operate as a **Network Intrusion Detection System (IDS)**.
* Npcap is required for live packet capture on Windows.
* Suricata must be connected to the correct network interface to monitor traffic.
* Installing Suricata alone does not guarantee that it can see the desired network traffic.
* The lab uses an isolated VirtualBox Host-Only network for controlled security testing.

## ✅ Lab Status

* [x] Npcap installed
* [x] Suricata installed
* [x] Suricata execution verified
* [x] Kali VM configured
* [x] Windows Host-Only IP identified
* [x] Kali IP identified
* [ ] Suricata network interface configured
* [ ] Live packet capture verified
* [ ] First Suricata alert generated

