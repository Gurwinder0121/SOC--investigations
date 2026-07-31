# SOC Investigation #7 — SQL Injection Attack
**Date:** July 19, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh
**Severity:** CRITICAL — Full Database Compromise

## Summary
Investigated a sophisticated SQL injection attack against a web 
application where an attacker bypassed authentication, stole all 
user credentials, attempted to destroy the database, created a 
persistent backdoor admin account, and exported the entire 
customer database — all within 14 minutes.

## Attack Overview
- **Attacker IP:** 192.168.1.200
- **Target:** Web application database
- **Entry Point:** SQL injection via login and search forms
- **Data Stolen:** All usernames/passwords + entire customer database
- **Persistence:** Fake admin account created (hacker/password123)

## Complete Attack Timeline

| Time | Action | Payload | Status |
|------|--------|---------|--------|
| 10:00:00 | Browsed login page | Normal GET request | 200 |
| 10:00:01 | Authentication bypass | username=admin'-- | 200 SUCCESS |
| 10:00:02 | Accessed dashboard | Normal navigation | 200 |
| 10:00:03 | Accessed users page | Normal navigation | 200 |
| 10:00:04 | Credential theft | UNION SELECT username,password FROM users | 200 SUCCESS |
| 10:00:05 | Data dump attempt | OR '1'='1' | 200 SUCCESS |
| 10:00:06 | Database destruction | DROP TABLE users | 500 FAILED |
| 10:00:07 | Backdoor created | INSERT INTO users VALUES hacker admin | 200 SUCCESS |
| 10:00:08 | Admin panel access | Normal navigation | 200 |
| 10:00:09 | Customer data export | SELECT * FROM customers | 200 SUCCESS |
| 10:00:10 | File downloaded | customers_export.csv | 200 |
| 10:00:12 | Normal user login | sarah normal login | 200 |
| 10:00:13 | Blind SQL test | SLEEP(5) timing attack | 200 |
| 10:00:14 | Attacker logged out | Normal logout | 200 |

## Attack Phases

**Phase 1 — Authentication Bypass**
Attacker entered username=admin'-- into the login form.
The -- comments out the rest of the SQL query including
the password check. Database logs the user in as admin
without verifying the password. Status 200 confirms success.

**Phase 2 — Credential Theft via UNION SELECT**
UNION SELECT attack combines attacker's query with the 
real database query. Result returns all usernames and 
passwords from the users table. Every account credential 
is now in the attacker's hands.

**Phase 3 — Database Destruction Attempt**
DROP TABLE users attempted to permanently delete the entire
users table. Returned status 500 — the attack failed.
However the intent was clearly destructive.

**Phase 4 — Persistence via Fake Admin Account**
INSERT INTO users created a new account:
Username: hacker
Password: password123
Role: admin
This backdoor survives even if the company resets all
existing passwords. Attacker can return anytime.

**Phase 5 — Full Data Exfiltration**
SELECT * FROM customers exported the entire customer
database including all personal and financial information.
Downloaded as customers_export.csv — status 200 confirms
successful theft.

**Phase 6 — Blind SQL Injection Testing**
SLEEP(5) command tests if the database responds to time
delays — used to confirm SQL injection vulnerability exists
even when no data is returned visibly.

## Splunk Queries Used
All POST requests containing attack payloads:
source="investigation7.txt" action=POST

Database destruction attempt:
source="investigation7.txt" payload="*DROP*"

Fake admin account creation:
source="investigation7.txt" payload="*INSERT*"

Data theft queries:
source="investigation7.txt" payload="*SELECT*"

## Critical Findings

**Finding 1 — Authentication Completely Bypassed**
Single quote and comment injection bypassed login entirely.
No password was needed to gain admin access.

**Finding 2 — All Passwords Stolen**
UNION SELECT successfully extracted all usernames and
passwords from the database. Every user account is compromised.

**Finding 3 — Persistent Backdoor Account**
Fake admin account hacker/password123 created successfully.
This is the most dangerous finding — the attacker has
permanent access that survives password resets.

**Finding 4 — Complete Customer Data Breach**
Entire customer database exported and downloaded.
Under Canadian PIPEDA law this requires breach notification
to affected customers and the Privacy Commissioner.

## Immediate Recommendations
1. Take the web application offline immediately
2. Delete the fake hacker admin account from database
3. Force password reset for ALL users — all credentials stolen
4. Implement parameterized queries to prevent SQL injection
5. Add Web Application Firewall (WAF) to detect SQL injection
6. Notify all customers of data breach under PIPEDA law
7. Audit all database accounts for other unauthorized accounts
8. Review all data accessed and downloaded during attack window
9. Implement input validation on all web forms

## Key Lesson
SQL injection is prevented entirely by using parameterized 
queries — a basic secure coding practice. This entire attack 
could have been prevented with proper input validation.
The single quote character ' should never be processed 
directly by the database.

## Comparison to Previous Investigations
Investigation 1 — Brute force, external attacker
Investigation 2 — Successful breach via username enumeration
Investigation 3 — Phishing, credential theft
Investigation 4 — Insider threat, corporate espionage
Investigation 5 — Network intrusion, lateral movement
Investigation 6 — Ransomware, file encryption
Investigation 7 — SQL injection, database compromise

## Skills Demonstrated
- SQL injection attack identification
- Authentication bypass detection
- UNION SELECT attack analysis
- Database destruction attempt detection
- Persistence mechanism identification
- Data exfiltration via SQL commands
- Wildcard Splunk searches using * operator
- Web application attack investigation
