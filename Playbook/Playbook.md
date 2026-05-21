# Incident Response Playbook: Bank Notification Phishing Attack

## 1. Preparation
- [Security Awareness Training](ca://s?q=Security_Awareness_Training) conducted quarterly for all employees.
- Enforce [SPF/DKIM/DMARC](ca://s?q=SPF_DKIM_DMARC_email_security) with strict rejection rules.
- Configure [SIEM Monitoring](ca://s?q=SIEM_monitoring) for suspicious login attempts.
- Define IR team roles and escalation channels.

## 2. Identification
- SIEM alert triggered by login from foreign IP.
- SOC analyzes email headers → spoofed domain detected (securebänk.com).
- Sandbox testing confirms malicious credential harvesting page.
- Incident severity assessed as **HIGH**.

## 3. Containment
- Block malicious domain via DNS blacklist.
- Isolate workstation WS-ACCT-042 from the network.
- Disable Sarah Chen’s account immediately.
- Update firewall to block attacker’s C2 IP (185.220.101.47).

## 4. Eradication
- Perform full antivirus scan on affected endpoint.
- Clear browser cache and temporary files.
- Reset credentials for Sarah and shared accounts.
- Attempt recall of phishing email from other inboxes.

## 5. Recovery
- Reimage workstation with clean gold image.
- Reinstate account with enforced [Multi-Factor Authentication](ca://s?q=Multi_Factor_Authentication).
- Enable enhanced monitoring for 72 hours.
- Verify business operations are back to normal.

## 6. Lessons Learned
- Root cause: Homograph attack bypassed visual inspection.
- Gap: No punycode detection in email gateway.
- Improvement: Deploy internationalized domain filtering.
- Metrics: MTTD 45 min, MTTC 90 min, MTTR 5 hrs.

## 7. MITRE ATT&CK Mapping
| Tactic             | Technique ID | Technique Name            | Context |
|--------------------|--------------|---------------------------|---------|
| Initial Access     | T1566.002    | Spearphishing Link        | Malicious email with fake bank link |
| Execution          | T1204.001    | User Execution            | Victim clicked the link |
| Credential Access  | T1003        | OS Credential Dumping     | Harvested login credentials |
| Collection         | T1005        | Data from Local System    | Downloaded account statements |
| Exfiltration       | T1041        | Exfiltration Over C2      | Sent credentials to attacker server |
| Defense Evasion    | T1036        | Masquerading              | Fake login page mimicked real bank |

## 8. Key Takeaways
1. [Homograph attacks](ca://s?q=Homograph_attack_explained) are highly effective against visual inspection.
2. Strict enforcement of SPF/DKIM/DMARC is essential.
3. User reporting remains a critical detection mechanism.
4. Rapid containment prevented financial loss.
5. MFA would have blocked account takeover even with stolen password.
