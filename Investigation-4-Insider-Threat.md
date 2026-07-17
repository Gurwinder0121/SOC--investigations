# SOC Investigation #4 — Insider Threat & Corporate Espionage
**Date:** July 16, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh
**Severity:** CRITICAL — Potential Criminal Activity

## Summary
Investigated suspicious employee activity revealing a serious insider 
threat involving data theft, corporate espionage, evidence tampering, 
and possible criminal conspiracy. Employee david.brown systematically 
stole sensitive company files and sent them to a competitor before 
an admin account was used to cover the tracks.

## Threat Actor
- **Primary Suspect:** david.brown (employee)
- **Secondary Suspect:** Unknown admin account user
- **Internal IP:** 10.0.0.55
- **External VPN IP:** 185.220.101.45

## Complete Attack Timeline

| Time | User | Action | Details |
|------|------|--------|---------|
| 09:00:00 | david.brown | login | Normal login from 10.0.0.55 |
| 09:01:00 | david.brown | file_accessed | client_contracts.xlsx |
| 09:02:00 | david.brown | file_accessed | financial_report_2026.xlsx |
| 09:03:00 | david.brown | file_accessed | employee_database.xlsx |
| 09:04:00 | david.brown | file_copied | client_contracts.xlsx → USB Drive |
| 09:05:00 | david.brown | file_copied | financial_report_2026.xlsx → USB Drive |
| 09:06:00 | david.brown | file_copied | employee_database.xlsx → USB Drive |
| 09:07:00 | david.brown | email_sent | client_contracts.xlsx → personal@gmail.com |
| 09:08:00 | david.brown | email_sent | financial_report_2026.xlsx → competitor@rivalcorp.com |
| 09:09:00 | david.brown | vpn_connected | External IP 185.220.101.45 |
| 09:10:00 | david.brown | file_uploaded | employee_database.xlsx → external server |
| 09:15:00 | david.brown | file_deleted | client_contracts.xlsx |
| 09:16:00 | david.brown | file_deleted | financial_report_2026.xlsx |
| 09:17:00 | david.brown | logout | Left the system |
| 09:20:00 | admin | login | Logged in 3 minutes after David left |
| 09:21:00 | admin | audit_log_cleared | DELETED ALL EVIDENCE |
| 09:22:00 | admin | user_account_deleted | Deleted david.brown account |
| 09:25:00 | sarah.jones | login | Normal employee — not involved |

## Attack Phases

**Phase 1 — Data Collection**
David accessed 3 sensitive files within 3 minutes suggesting 
he knew exactly what he was looking for. This was planned not accidental.

**Phase 2 — Data Exfiltration via Multiple Channels**
David used 3 different methods to steal data:
- Physical: Copied files to USB drive
- Email: Sent to personal Gmail and competitor email
- Network: Uploaded via VPN to external server
Using multiple channels ensures data theft even if one method is blocked.

**Phase 3 — Corporate Espionage**
Sending financial_report_2026.xlsx directly to competitor@rivalcorp.com
is corporate espionage. This gives a competitor insider knowledge of
company finances, strategies, and client relationships.

**Phase 4 — Evidence Destruction**
David deleted original files after copying them — attempting to 
hide that the files existed or were accessed.

**Phase 5 — Cover Up by Admin Account**
Three minutes after David logged out, an admin account:
- Cleared ALL audit logs — attempted to erase investigation trail
- Deleted David's user account — removed primary suspect from system
This strongly suggests an accomplice with admin privileges or 
David himself had admin access.

## Critical Findings

**Finding 1 — Triple Exfiltration**
Data was stolen via USB, email, AND external server upload.
Even if one channel was blocked, two others succeeded.

**Finding 2 — Corporate Espionage Confirmed**
Direct email to competitor with confidential financial data.
This is potentially criminal under Canadian law.

**Finding 3 — Evidence Tampering**
Audit logs were deliberately cleared to obstruct investigation.
However Splunk captured the events before clearing occurred.

**Finding 4 — Possible Criminal Conspiracy**
Admin account activity immediately after David's logout suggests
coordination between David and someone with admin privileges.

## Splunk Queries Used
All events:
source="investigation4.txt"

David Brown complete activity:
source="investigation4.txt" user=david.brown

Corporate espionage evidence:
source="investigation4.txt" action=email_sent

Admin coverup activity:
source="investigation4.txt" user=admin

## Immediate Recommendations
1. Preserve all Splunk logs immediately before further tampering
2. Escalate to senior management and legal team immediately
3. Contact law enforcement — criminal activity suspected
4. Identify who used the admin account at 09:20:00
5. Contact competitor@rivalcorp.com's company legal team
6. Notify clients whose contracts were stolen
7. Revoke all admin credentials and audit who has admin access
8. Recover deleted files from backup systems
9. Forensically examine David's work computer and USB drives
10. Review all admin account activity for past 90 days

## Comparison to Previous Investigations
Investigation 1 — External brute force, attacker failed
Investigation 2 — External brute force, attacker succeeded
Investigation 3 — Phishing attack, external attacker
Investigation 4 — Insider threat, most dangerous type
Insider threats are most dangerous because the attacker already 
has legitimate access and knows exactly where sensitive data is stored.

## Key Lesson
Audit log tampering almost worked. If this company relied only on 
their internal logs they would have found nothing. This is why 
external SIEM tools like Splunk that capture logs independently 
are critical — they preserve evidence even when internal logs are cleared.

## Skills Demonstrated
- Insider threat detection and investigation
- Corporate espionage identification
- Evidence tampering detection
- Multi-channel data exfiltration analysis
- Criminal activity escalation procedures
- Advanced Splunk log correlation
