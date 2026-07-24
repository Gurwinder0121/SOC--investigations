# SOC Investigation #5 — Network Intrusion & Lateral Movement
**Date:** July 17, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh
**Severity:** CRITICAL — Full Network Compromise

## Summary
Investigated a sophisticated network intrusion where an external 
attacker exploited an SSH vulnerability to gain access to the 
network, moved laterally across three internal systems, installed 
multiple malware types, exfiltrated sensitive data, and installed 
a backdoor for persistent access.

## Attacker Information
- **Attacker IP:** 185.220.101.45 (External)
- **Target Network:** 192.168.1.0/24
- **Systems Compromised:** 4 (firewall + 3 internal systems)
- **Entry Point:** SSH vulnerability on port 22
- **Backdoor Port:** 4444

## Complete Attack Timeline

| Time | Action | Details |
|------|--------|---------|
| 10:00:00 | Port Scan | Scanned ports 22,80,443,3389,8080 |
| 10:00:30 | Vulnerability Found | SSH vulnerability on port 22 |
| 10:01:00 | Exploit Attempted | SSH exploit launched |
| 10:01:30 | Access Gained | Successfully entered network |
| 10:02:00 | Lateral Movement | Moved to 192.168.1.10 via SMB |
| 10:02:30 | Lateral Movement | Moved to 192.168.1.20 via SMB |
| 10:03:00 | Lateral Movement | Moved to 192.168.1.30 via SMB |
| 10:03:30 | Malware Installed | keylogger.exe on 192.168.1.10 |
| 10:04:00 | Malware Installed | ransomware.exe on 192.168.1.20 |
| 10:04:30 | Malware Installed | spyware.exe on 192.168.1.30 |
| 10:05:00 | Data Exfiltrated | passwords.txt (2MB) from .10 |
| 10:05:30 | Data Exfiltrated | financial_data.xlsx (15MB) from .20 |
| 10:06:00 | Data Exfiltrated | customer_records.xlsx (28MB) from .30 |
| 10:06:30 | Backdoor Installed | Port 4444 on main server |
| 10:07:00 | Normal Traffic | Internal users unaffected |
| 10:08:00 | Backdoor Accessed | Attacker returned via port 4444 |
| 10:08:30 | Logs Cleared | Attempted evidence destruction |
| 10:09:00 | Connection Terminated | Attacker disconnected |

## Attack Phases

**Phase 1 — Reconnaissance**
Attacker scanned 5 common ports to identify running services
and find potential vulnerabilities before launching attack.

**Phase 2 — Initial Access**
SSH vulnerability on port 22 was exploited to gain initial
foothold on the network gateway at 192.168.1.1

**Phase 3 — Lateral Movement**
Using SMB protocol attacker moved to 3 internal systems
within 1 minute. SMB was chosen because it is normal
internal network traffic and less likely to trigger alerts.

**Phase 4 — Malware Deployment**
Three different malware types were strategically deployed:
- Keylogger on .10 — steal credentials for future access
- Ransomware on .20 — potential financial extortion
- Spyware on .30 — ongoing surveillance of activities

**Phase 5 — Data Exfiltration**
Total of 45MB of sensitive data stolen across 3 systems:
- passwords.txt — credential database
- financial_data.xlsx — financial records
- customer_records.xlsx — customer personal data

**Phase 6 — Persistence**
Backdoor installed on port 4444 and immediately accessed
confirming it was functional. Attacker now has permanent
re-entry capability regardless of password changes.

**Phase 7 — Evidence Destruction**
Logs cleared before disconnecting — standard attacker
cleanup procedure to obstruct investigation.

## Splunk Queries Used
Port scanning activity:
source="investigation5.txt" action=port_scan

Lateral movement:
source="investigation5.txt" action=lateral_movement

Malware installation:
source="investigation5.txt" action=malware_installed

Backdoor activity:
source="investigation5.txt" action=backdoor_installed OR action=backdoor_accessed

## Critical Recommendations
1. Immediately isolate all affected systems from network
2. Block IP 185.220.101.45 at firewall immediately
3. Patch SSH vulnerability on port 22 immediately
4. Scan all systems for and remove all malware
5. Find and close backdoor on port 4444
6. Reset ALL passwords across entire network
7. Check if ransomware has been activated on 192.168.1.20
8. Notify customers whose records were stolen
9. Conduct full forensic investigation on all 4 systems
10. Implement network segmentation to prevent future lateral movement
11. Enable IDS/IPS to detect port scanning and SMB lateral movement

## Key Technical Concepts Demonstrated
**Port Scanning** — attacker maps network to find vulnerabilities
**SSH Exploitation** — using known vulnerability to gain access
**Lateral Movement via SMB** — moving between internal systems
**Malware Deployment** — installing different tools for different purposes
**Data Exfiltration** — stealing data from multiple systems
**Backdoor/Persistence** — maintaining permanent access
**Log Tampering** — destroying evidence

## Comparison to Previous Investigations
Investigation 1 — Single user brute force, external attacker
Investigation 2 — Single system breach, external attacker
Investigation 3 — Phishing, social engineering attack
Investigation 4 — Insider threat, internal attacker
Investigation 5 — Full network intrusion, most sophisticated yet
Each investigation has shown increasing attacker sophistication.

## Skills Demonstrated
- Network traffic analysis
- Port scan detection
- Lateral movement identification
- Multi-system malware analysis
- Backdoor detection
- Full network compromise investigation
- Advanced Splunk query writing with OR operators
