# Lab 01: Detection Engineering — From Requirement to Detection

## Aim
To understand how to convert a security requirement into a detection rule using logs and SIEM searches.

## Objectives
- Identify the security activity to detect.
- Determine the required log sources and fields.
- Develop detection logic using Splunk SPL.
- Test the detection and analyze its results.
- Understand false positives and detection limitations.

## Tools Used
- Splunk Enterprise
- Windows Event Logs
- Sysmon
- Suricata (where required)

## Methodology
1. Define the security detection requirement.
2. Identify relevant log sources and event fields.
3. Design the detection logic.
4. Execute the search in Splunk.
5. Validate the results using test activity and supporting evidence.
6. Document findings, false positives, and limitations.

## Expected Outcome
Understand the detection engineering workflow and learn how to transform raw security events into meaningful alerts for SOC L1 investigations.

## Key Learning
A detection rule should be based on a clear security requirement, relevant telemetry, testable logic, and validated results. An alert alone does not confirm an attack.

