# SOC Investigation #1 — Brute Force Attack
**Date:** July 8, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh

## Summary
Investigated suspicious activity in web server logs using Splunk. 
Identified a brute force attack followed by privilege escalation attempt 
from a single IP address.

## Timeline of Attack

| Time | IP Address | Action | Response |
|------|-----------|--------|----------|
| 01:00:01 - 01:00:05 | 192.168.1.105 | GET /login.php | 200 (browsing) |
| 01:00:06 - 01:00:10 | 192.168.1.105 | GET /login.php | 401 (failed login x5) |
| 01:00:12 - 01:00:14 | 192.168.1.105 | GET /admin.php | 403 (forbidden x3) |
| 01:20:00 - 01:20:02 | 192.168.1.105 | GET /passwd.php | 403 (forbidden x3) |

## Attack Pattern Identified
**Phase 1 — Reconnaissance:** Attacker browsed login page normally
**Phase 2 — Brute Force:** 5 failed login attempts in 5 seconds
**Phase 3 — Privilege Escalation:** Attempted direct access to admin panel
**Phase 4 — Credential Access:** Attempted access to password file

## Splunk Queries Used
Search all events:
source="attacks_logs.txt"

Search failed logins:
source="attacks_logs.txt" 401

Search forbidden access:
source="attacks_logs.txt" 403

Search all attacker activity:
source="attacks_logs.txt" 192.168.1.105

## Conclusion
IP address 192.168.1.105 conducted a brute force attack against 
the login page followed by direct access attempts to sensitive 
admin and password files. 

Recommended action: Block IP 192.168.1.105 immediately and 
implement account lockout policy after 3 failed login attempts.

## Skills Demonstrated
- Log analysis using Splunk
- Threat detection and identification
- Attack pattern recognition
- Incident documentation
