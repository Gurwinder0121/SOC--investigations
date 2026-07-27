# SOC Investigation #6 — Ransomware Attack
**Date:** July 18, 2026
**Tool Used:** Splunk Enterprise
**Analyst:** Gurwinder Singh
**Severity:** CRITICAL — Full Company Encryption

## Summary
Investigated a devastating ransomware attack that began with a 
single employee opening a malicious email attachment and resulted 
in complete encryption of 4 systems including the main file server, 
deletion of all backups, and a $50,000 Bitcoin ransom demand — 
all within 4 minutes.

## Attack Overview
- **Patient Zero:** mike.jones on WORKSTATION-01
- **Initial Vector:** Malicious email attachment Invoice_2026.pdf.exe
- **Systems Encrypted:** 4 (3 workstations + file server)
- **Ransom Demand:** $50,000 in Bitcoin
- **Bitcoin Wallet:** 1A2B3C4D5E
- **Time to Full Encryption:** 4 minutes

## Complete Attack Timeline

| Time | Host | Action | Details |
|------|------|--------|---------|
| 08:00:00 | WORKSTATION-01 | email_opened | Fake invoice from billing@inv01ce-corp.com |
| 08:00:30 | WORKSTATION-01 | attachment_opened | Invoice_2026.pdf.exe launched by mike.jones |
| 08:00:31 | WORKSTATION-01 | process_created | Ransomware process started via outlook.exe |
| 08:00:32 | WORKSTATION-01 | registry_modified | Added to startup registry for persistence |
| 08:00:33 | WORKSTATION-01 | network_connection | Connected to attacker C2 server 185.220.101.45 |
| 08:00:34 | WORKSTATION-01 | encryption_started | Files targeted: .docx .xlsx .pdf .jpg |
| 08:00:35 | WORKSTATION-01 | file_renamed | report.docx → report.docx.locked |
| 08:00:36 | WORKSTATION-01 | file_renamed | salary.xlsx → salary.xlsx.locked |
| 08:00:37 | WORKSTATION-01 | file_renamed | customer_data.pdf → customer_data.pdf.locked |
| 08:00:38 | WORKSTATION-01 | ransom_note_created | README_DECRYPT.txt created |
| 08:01:00 | WORKSTATION-02 | encryption_started | Ransomware spread to second workstation |
| 08:01:30 | WORKSTATION-03 | encryption_started | Ransomware spread to third workstation |
| 08:02:00 | FILE-SERVER-01 | encryption_started | Main file server encrypted including .db files |
| 08:02:30 | FILE-SERVER-01 | backup_deletion_attempted | Targeted company backup server |
| 08:03:00 | FILE-SERVER-01 | backup_deleted | Company backups destroyed |
| 08:03:30 | WORKSTATION-01 | ransom_demand | $50,000 Bitcoin wallet 1A2B3C4D5E |
| 08:04:30 | FILE-SERVER-01 | shadow_copies_deleted | Last recovery option destroyed |
| 08:05:00 | WORKSTATION-01 | ransom_note_opened | mike.jones reads ransom demand |

## Attack Phases

**Phase 1 — Initial Infection**
Attacker sent fake invoice email from spoofed domain inv01ce-corp.com
using number 1 instead of letter i to impersonate a legitimate company.
Attachment appeared to be a PDF but was actually an executable (.exe) file.
Mike Jones opened it believing it was a real invoice.

**Phase 2 — Persistence and C2 Connection**
Within 2 seconds of opening the ransomware:
- Modified Windows registry to survive system restarts
- Connected to attacker's Command and Control server at 185.220.101.45
- Received encryption keys from attacker server

**Phase 3 — Encryption**
Ransomware encrypted all .docx .xlsx .pdf .jpg and .db files
across all connected systems. Files renamed with .locked extension.
Ransom note README_DECRYPT.txt dropped on WORKSTATION-01.
Total time to encrypt all 4 systems — under 2 minutes.

**Phase 4 — Backup Destruction**
Ransomware specifically targeted and deleted:
- Network backup server (\\backup-server\company-backup)
- Windows Shadow Copies on all systems
This eliminated all recovery options except paying ransom.

**Phase 5 — Ransom Demand**
$50,000 demanded in Bitcoin to wallet 1A2B3C4D5E.
Bitcoin chosen because transactions cannot be reversed
and are extremely difficult to trace to the attacker.

## Critical Findings

**Finding 1 — Double Extension Trick**
Invoice_2026.pdf.exe uses double extension to disguise executable
as PDF. Windows hides extensions by default making this very effective.

**Finding 2 — Speed of Spread**
4 minutes from infection to complete company encryption.
By the time IT notices something is wrong it is already too late.

**Finding 3 — Backup Targeting**
Modern ransomware specifically searches for and deletes backups.
Connected backups provide zero protection against ransomware.

**Finding 4 — Registry Persistence**
Ransomware added to Windows startup registry meaning it survives
reboots and continues encrypting any new files created.

**Finding 5 — C2 Communication**
Multiple connections to 185.220.101.45 show ransomware was
communicating with attacker server throughout the attack.

## Splunk Queries Used
Patient zero identification:
source="investigation6.txt" action=attachment_opened

Encryption spread:
source="investigation6.txt" action=encryption_started

Backup destruction:
source="investigation6.txt" action=backup_deleted OR action=shadow_copies_deleted

Ransom demand:
source="investigation6.txt" action=ransom_demand

## Immediate Response Recommendations
1. Isolate ALL systems from network immediately
2. Do NOT pay the ransom — no guarantee of decryption
3. Contact law enforcement — FBI and RCMP have ransomware units
4. Check for any offline backups not connected to network
5. Preserve all logs for forensic investigation
6. Identify and block C2 server 185.220.101.45 at firewall
7. Notify affected customers — data may have been stolen before encryption
8. Engage professional ransomware incident response team

## Prevention Recommendations
1. Never open email attachments from unknown senders
2. Enable file extension visibility in Windows
3. Keep offline backups completely disconnected from network
4. Implement email filtering to block .exe attachments
5. Deploy endpoint detection and response (EDR) tools
6. Train all employees on phishing and malicious attachment awareness
7. Implement application whitelisting to prevent unauthorized executables
8. Regular security awareness training especially around invoice fraud

## Key Lesson
The most important lesson from this investigation is the 3-2-1 backup rule:
- 3 copies of data
- 2 different storage types
- 1 copy completely offline and disconnected from network
If the company had followed this rule the ransomware attack would have
been painful but recoverable without
