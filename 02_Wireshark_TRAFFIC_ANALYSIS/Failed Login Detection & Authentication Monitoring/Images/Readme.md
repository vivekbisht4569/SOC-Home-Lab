# 🔍 Detection & Investigation

Investigated Windows Security Logs using:

```text
Event Viewer
→ Windows Logs
→ Security
```

Filtered Logs:

```text
Event ID: 4625
```

---

# 📌 Important Findings

| Field | Value |
|---|---|
| Event ID | 4625 |
| Target Username | Administrator |
| Logon Type | 3 (Network Login) |
| Authentication Package | NTLM |
| Attacker IP | 192.168.56.101 |
| Status Code | 0xc000006d |
| SubStatus | 0xc000006a |

---

# 🧠 Analysis

The logs confirmed a failed SMB authentication attempt originating from the Kali Linux attacker machine (`192.168.56.101`) targeting the `Administrator` account on the Windows host.

The authentication attempt used:
- NTLM authentication
- Network-based logon (Logon Type 3)

The status and substatus codes indicated:
- authentication failure
- incorrect password attempt

This behavior is commonly associated with:
- brute-force attacks
- failed remote authentication attempts
- suspicious SMB login activity

This investigation helped demonstrate how SOC analysts detect and analyze authentication-based attack telemetry using Windows Security Logs.