# Lab 04 — TCP Port Reconnaissance Detection & Investigation

## Objective

The objective of this lab was to monitor TCP connection attempts from a Kali Linux machine to a Windows host using Suricata and investigate the collected telemetry using PowerShell.

This lab focused on understanding network traffic, analyzing destination ports, extracting fields from EVE JSON logs, and identifying activity consistent with TCP port reconnaissance.

## Lab Environment

| Component | Details |
|---|---|
| Attacker / Traffic Generator | Kali Linux |
| Target / IDS Sensor | Windows |
| IDS | Suricata 8.0.6 |
| Packet Analysis | TShark |
| Log Analysis | PowerShell |
| Source IP | `192.168.56.103` |
| Destination IP | `192.168.56.1` |
| Protocol | TCP |

## Architecture

`Kali Linux → Host-Only Network → Npcap → Suricata → EVE JSON / fast.log → PowerShell Investigation`

## Step 1 — Configure the Detection Rule

The following rule was added to:

`C:\Suricata\rules\local.rules`

    alert tcp 192.168.56.103 any -> 192.168.56.1 any (msg:"KALI_TCP_CONNECTION_ATTEMPT"; sid:1000005; rev:1;)

### Rule Explanation

- `alert tcp` — generates an alert when matching TCP traffic is detected.
- `192.168.56.103 any` — matches traffic from Kali using any source port.
- `->` — specifies the traffic direction.
- `192.168.56.1 any` — matches traffic destined for the Windows host on any destination port.
- `sid:1000005` — identifies the local detection rule.
- `rev:1` — indicates the rule revision.

The rule detects matching TCP packets. It does not independently correlate multiple packets to identify a port scan.

## Step 2 — Validate the Configuration

The Suricata configuration was tested before starting the detection process.

    C:\Suricata\suricata.exe -T -c C:\Suricata\suricata.yaml

A successful configuration test confirms that Suricata can load the configuration and rules without reporting a configuration error.

## Step 3 — Generate TCP Traffic

From the Kali Linux terminal, a controlled TCP connect scan was performed against the Windows host.

    nmap -sT -p 80,135,139,445,3389 192.168.56.1

### Command Explanation

- `nmap` — network scanning tool.
- `-sT` — performs a TCP connect scan.
- `-p` — specifies the destination ports to test.
- `192.168.56.1` — identifies the Windows lab target.

Only the specified ports on the user's isolated lab target were tested.

## Step 4 — Review Suricata Alerts

The following PowerShell command was used to inspect recent alerts:

    Get-Content C:\Suricata\log\fast.log -Tail 20

The structured EVE JSON log was also inspected:

    Get-Content C:\Suricata\log\eve.json -Tail 100

The EVE JSON log provides structured network telemetry that can be queried for source IPs, destination IPs, ports, protocols, timestamps, and event types.

## Step 5 — Investigate TCP Events Using PowerShell

The following pipeline was used to filter TCP events involving Kali and Windows:

    Get-Content C:\Suricata\log\eve.json -Tail 500 |
    ForEach-Object { $_ | ConvertFrom-Json } |
    Where-Object {
        $_.src_ip -eq "192.168.56.103" -and
        $_.dest_ip -eq "192.168.56.1" -and
        $_.proto -eq "TCP"
    } |
    Select-Object timestamp, src_ip, dest_ip, dest_port, event_type

### PowerShell Concepts Practiced

- `Get-Content` — reads the log file.
- `ForEach-Object` — processes each line.
- `$_` — represents the current pipeline item.
- `ConvertFrom-Json` — converts JSON text into PowerShell objects.
- `Where-Object` — filters events using conditions.
- `-eq` — checks equality.
- `-and` — requires multiple conditions to be true.
- `Select-Object` — selects the fields to display.

The investigation followed this workflow:

**Read → Convert → Filter → Select → Investigate**

## Step 6 — Count Unique Destination Ports

The following command extracted unique destination ports from the matching TCP events:

    $ports = Get-Content C:\Suricata\log\eve.json -Tail 500 |
    ForEach-Object { $_ | ConvertFrom-Json } |
    Where-Object {
        $_.src_ip -eq "192.168.56.103" -and
        $_.dest_ip -eq "192.168.56.1" -and
        $_.proto -eq "TCP"
    } |
    Select-Object -ExpandProperty dest_port -Unique

    $ports.Count

### Explanation

- `-ExpandProperty dest_port` — outputs the destination port values.
- `-Unique` — removes duplicate values from the resulting output.
- `$ports` — stores the unique port values.
- `.Count` — returns the number of items in the collection.

The count represents unique destination port values present in the selected log records, not the total number of packets or connection attempts.

## Step 7 — Group Events by Destination Port

The following query was used to examine how many matching events appeared for each destination port:

    Get-Content C:\Suricata\log\eve.json -Tail 500 |
    ForEach-Object { $_ | ConvertFrom-Json } |
    Where-Object {
        $_.src_ip -eq "192.168.56.103" -and
        $_.dest_ip -eq "192.168.56.1" -and
        $_.proto -eq "TCP"
    } |
    Group-Object dest_port |
    Select-Object Name, Count

`Group-Object dest_port` groups records with the same destination port, while `Count` shows how many matching records belong to each group.

## Observations

During the investigation:

- TCP connection attempts were generated from Kali toward the Windows lab host.
- Five destination ports were examined: `80`, `135`, `139`, `445`, and `3389`.
- The observed flow records included activity involving these destination ports.
- PowerShell was used to filter events, extract fields, count unique destination ports, and group events by port.

The exact counts depend on the events present in the selected log window. The `-Tail 500` option limits the investigation to the latest 500 lines.

## SOC Analyst Assessment

**Finding:** Multiple TCP destination ports were contacted by the Kali lab machine on the Windows host.

**Assessment:** The observed activity is consistent with port-scan-like reconnaissance and warrants further investigation.

**Limitations:** Multiple port attempts alone do not prove malicious intent or system compromise. Additional evidence, including packet-level TCP flags, connection outcomes, timestamps, and relevant alerts, is needed for a stronger conclusion.

## Key Learnings

1. Created and validated a Suricata TCP detection rule.
2. Generated controlled TCP traffic using Nmap.
3. Investigated Suricata telemetry through EVE JSON.
4. Practiced JSON conversion and field selection in PowerShell.
5. Used filtering conditions to isolate traffic between two lab machines.
6. Counted unique destination ports and grouped events by port.
7. Distinguished observed network activity from a confirmed security incident.

## Conclusion

This lab demonstrated a basic workflow for TCP reconnaissance investigation using Suricata and PowerShell. It connected network traffic generation, IDS monitoring, structured log analysis, and evidence-based SOC assessment.

The next step is to investigate TCP flags, particularly SYN packets, to improve understanding of connection attempts and reconnaissance behavior.