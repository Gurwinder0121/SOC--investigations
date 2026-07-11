# SOC Investigation #2 — Successful Breach via Username Enumeration
**Date:** July 11, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh

## Summary
Investigated a multi-stage attack in web server logs using Splunk.
Identified a complete attack chain including reconnaissance,
username enumeration brute force, successful authentication bypass,
and post-breach data theft — all from a single attacker IP.

## Attacker Information
- **Attacker IP:** 192.168.1.200
- **Target:** Web application login system
- **Result:** SUCCESSFUL BREACH — data accessed

## Complete Attack Timeline

| Time | Action | Response | Phase |
|------|--------|----------|-------|
| 00:01:00 | GET /index.php | 200 | Reconnaissance |
| 00:01:01 | GET /admin.php | 200 | Reconnaissance |
| 00:01:02 | GET /login.php | 200 | Reconnaissance |
| 00:01:03 | GET /config.php | 404 | Scanning |
| 00:01:04 | GET /backup.php | 404 | Scanning |
| 00:01:05 | GET /db.php | 404 | Scanning |
| 00:01:06 | POST /login.php user=admin | 401 | Brute Force |
| 00:01:07 | POST /login.php user=administrator | 401 | Brute Force |
| 00:01:08 | POST /login.php user=root | 401 | Brute Force |
| 00:01:09 | POST /login.php user=test | 401 | Brute Force |
| 00:01:10 | POST /login.php user=gurwinder | 401 | Brute Force |
| 00:01:11 | POST /login.php user=admin123 | 200 | BREACH |
| 00:01:12 | GET /dashboard.php | 200 | Post-Breach |
| 00:01:13 | GET /users.php | 200 | Data Theft |
| 00:01:14 | GET /passwords.php | 200 | Data Theft |
| 00:01:15 | GET /database.php | 200 | Data Theft |

## Attack Phases Identified

**Phase 1 — Reconnaissance**
Attacker browsed the website normally to map available pages
and understand the application structure before attacking.

**Phase 2 — Scanning**
Attacker attempted to access hidden sensitive files
(config, backup, database files). All returned 404 not found
but revealed the attacker's intent to find vulnerabilities.

**Phase 3 — Username Enumeration Brute Force**
Attacker systematically tried 6 common usernames in 5 seconds.
This is more sophisticated than password brute force as it
targets finding valid usernames first.
Usernames tried: admin, administrator, root, test, gurwinder, admin123

**Phase 4 — Successful Breach**
Username admin123 with matching password successfully authenticated
at 00:01:11. Attacker gained full access to the application.

**Phase 5 — Post-Breach Data Theft**
Within seconds of gaining access attacker accessed:
- /dashboard.php — administrative dashboard
- /users.php — user account database
- /passwords.php — password storage
- /database.php — full database access

## Splunk Queries Used
All events from investigation:
source="Investigation2.txt"

Reconnaissance scanning (404 errors):
source="Investigation2.txt" 404

Failed login attempts (401 errors):
source="Investigation2.txt" 401

Successful breach (200 responses):
source="Investigation2.txt" 200

Complete attacker activity:
source="Investigation2.txt" 192.168.1.200

## Recommendations
1. Immediately block IP address 192.168.1.200
2. Force password reset on admin123 account
3. Implement account lockout after 3 failed attempts
4. Enable Multi Factor Authentication on all admin accounts
5. Remove or restrict access to /passwords.php and /database.php
6. Implement Web Application Firewall to detect scanning behavior
7. Review all data accessed between 00:01:12 and 00:01:15

## Comparison to Investigation 1
Investigation 1 — Attacker failed, never got in
Investigation 2 — Attacker succeeded, data was stolen
This investigation is more serious and required more urgent response.

## Skills Demonstrated
- Multi-phase attack recognition
- Username enumeration detection
- Successful breach identification
- Post-breach activity analysis
- Incident documentation and recommendations
- Splunk SPL query writing
