# SOC Investigation #3 — Phishing Attack Leading to Data Breach
**Date:** July 13, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh

## Summary
Investigated a phishing campaign targeting company employees.
Identified a multi-stage attack where a fake email led to
credential theft and ultimately a serious data breach affecting
customer and employee data.

## Attack Overview
- **Attack Type:** Phishing → Credential Theft → Data Breach
- **Attacker IP:** 192.168.1.200
- **Phishing Domain:** security@paypa1.com (fake PayPal domain)
- **Employees Targeted:** 3
- **Employees Compromised:** 1 (john.smith)
- **Data Stolen:** 2 sensitive files

## Phishing Email Details
- **From:** security@paypa1.com
- **Subject:** "Urgent: Verify Your Account"
- **Malicious URL:** http://paypa1.com/verify
- **Red Flag:** paypa1.com uses number 1 instead of letter L
to impersonate paypal.com

## Employee Response Analysis

| Employee | Opened Email | Clicked Link | Entered Credentials | Compromised |
|----------|-------------|--------------|--------------------| ------------|
| john.smith | Yes 08:00:01 | Yes 08:00:02 | Yes 08:00:03 | YES |
| sarah.jones | Yes 08:01:00 | Yes 08:01:01 | No | NO |
| mike.wilson | Yes 08:02:00 | No | No | NO |

## Complete Attack Timeline

| Time | User | Action | Details |
|------|------|--------|---------|
| 08:00:01 | john.smith | email_opened | Phishing email received |
| 08:00:02 | john.smith | link_clicked | Clicked malicious URL |
| 08:00:03 | john.smith | credentials_entered | Typed password on fake site |
| 08:00:04 | john.smith | password_changed | Attacker changed password |
| 08:01:00 | sarah.jones | email_opened | Phishing email received |
| 08:01:01 | sarah.jones | link_clicked | Clicked but did not enter credentials |
| 08:02:00 | mike.wilson | email_opened | Opened but did not click |
| 08:05:00 | admin | login_failed | Attacker tried admin account |
| 08:05:03 | john.smith | login_success | Attacker logged in as John from 192.168.1.200 |
| 08:05:04 | john.smith | file_accessed | employee_salaries.xlsx |
| 08:05:05 | john.smith | file_accessed | customer_database.xlsx |
| 08:05:06 | john.smith | file_downloaded | customer_database.xlsx STOLEN |
| 08:05:07 | john.smith | file_downloaded | employee_salaries.xlsx STOLEN |
| 08:10:00 | sarah.jones | login_success | Normal login from internal IP |
| 08:15:00 | mike.wilson | email_deleted | Correctly deleted phishing email |
| 08:20:00 | john.smith | logout | Attacker finished and disconnected |

## Attack Phases

**Phase 1 — Phishing Campaign**
Attacker sent fake urgent email from paypa1.com impersonating
PayPal to create panic and urgency. Three employees received it.

**Phase 2 — Credential Harvesting**
John clicked link and entered credentials on fake website.
Sarah clicked but got suspicious and did not enter credentials.
Mike recognized it as suspicious and deleted it immediately.

**Phase 3 — Account Takeover**
Attacker used John's stolen credentials to login from
192.168.1.200 at 08:05:03. Password was already changed
at 08:00:04 so John could no longer access his own account.

**Phase 4 — Data Theft**
Within 4 minutes of gaining access attacker downloaded
two highly sensitive files containing customer and employee data.

## Data Breach Assessment
**Severity: CRITICAL**
- customer_database.xlsx — contains personal data of all customers
- employee_salaries.xlsx — contains confidential HR information
- Under Canadian PIPEDA law this breach must be reported to
the Privacy Commissioner within 72 hours

## Splunk Queries Used
All phishing email opens:
source="investigation3.txt" action=email_opened

Link clicks:
source="investigation3.txt" action=link_clicked

Credential theft:
source="investigation3.txt" action=credentials_entered

Files downloaded:
source="investigation3.txt" action=file_downloaded

Complete compromised account activity:
source="investigation3.txt" user=john.smith

## Recommendations
1. Immediately disable john.smith account
2. Block IP address 192.168.1.200
3. Force password reset for sarah.jones as precaution
4. Notify affected customers of potential data breach
5. Report breach to Privacy Commissioner under PIPEDA within 72 hours
6. Implement email filtering to block domains impersonating known brands
7. Deploy Multi Factor Authentication so stolen passwords alone cannot grant access
8. Conduct urgent security awareness training for all employees
9. Investigate what attacker did with stolen data

## Key Lessons Learned
- Urgency in emails is a major red flag — attackers create panic
- Domain spoofing is subtle — paypa1.com vs paypal.com
- MFA would have prevented this breach entirely
- One employee Mike Wilson demonstrated good security awareness
- Even clicking a link without entering credentials kept Sarah safe

## Comparison to Previous Investigations
Investigation 1 — External brute force, attacker failed
Investigation 2 — External brute force, attacker succeeded
Investigation 3 — Phishing attack, insider credential theft, data stolen
Each investigation shows different attack type and increasing complexity.

## Skills Demonstrated
- Phishing attack identification and analysis
- Credential theft detection
- Account takeover investigation
- Data breach assessment
- Canadian privacy law awareness (PIPEDA)
- Multi-source log correlation
- Incident documentation and recommendations
