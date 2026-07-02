#  SPL Queries Used (Methodology Behind SPL Queries)

## 1. Verify Available Log Sources

```spl
index=*
| stats count by source
```

**Purpose:** Verify which log sources are available in Splunk before starting an investigation.

---

## 2. Explore Windows Security Logs

```spl
source="WinEventLog:Security"
| table _time EventCode Account_Name Logon_Type ComputerName
```

**Purpose:** Display Windows Security events and inspect important fields.

---

## 3. Investigate Successful Logins (Event ID 4624)

```spl
source="WinEventLog:Security" EventCode=4624
| table _time Account_Name Logon_Type Workstation_Name Source_Network_Address
```

**Purpose:** Identify successful authentication events.

---

## 4. Investigate Failed Logins (Event ID 4625)

```spl
source="WinEventLog:Security" EventCode=4625
| table _time Account_Name Failure_Reason Source_Network_Address
```

**Purpose:** Detect failed authentication attempts and potential brute-force activity.

---

## 5. Count Successful Logins by User

```spl
source="WinEventLog:Security" EventCode=4624
| stats count by Account_Name
```

**Purpose:** Identify the most active accounts.

---

## 6. Analyze Logon Types

```spl
source="WinEventLog:Security" EventCode=4624
| stats count by Logon_Type
```

**Purpose:** Understand how users authenticated (Interactive, Service, Network, etc.).

---

## 7. Authentication Timeline

```spl
source="WinEventLog:Security" EventCode=4624
| timechart span=1m count
```

**Purpose:** Visualize authentication activity over time.

---

## 8. Discover Available Windows Security Event IDs

```spl
source="WinEventLog:Security"
| stats count by EventCode
| sort EventCode
```

**Purpose:** Identify the Windows Security Event IDs available for investigation.

---

## 9. Display All Authentication Events

```spl
source="WinEventLog:Security" (EventCode=4624 OR EventCode=4625)
| table _time EventCode Account_Name Logon_Type ComputerName
```

**Purpose:** View successful and failed authentication events together for comparison.

---

## 10. Identify Authentication Event Distribution

```spl
source="WinEventLog:Security"
| stats count by EventCode
```

**Purpose:** Understand which authentication and security events occur most frequently.