# 🎯 Simulation Scenario: Bank Notification Phishing Attack

## 1. Scenario Overview

| Attribute | Details |
|-----------|---------|
| **Organization** | Mid-Size Financial Firm (FinServe Corp.) |
| **Industry** | Banking & Financial Services |
| **Target** | Junior Accountant (Sarah Chen) |
| **Attack Vector** | Spear Phishing Email |
| **Threat Actor** | APT Group "SILVER PHANTOM" (Assumed) |
| **MITRE ATT&CK** | T1566.001 (Spearphishing Attachment), T1566.002 (Spearphishing Link) |
| **Duration** | 09:00 - 14:00 (5 Hours) |
| **Impact** | Credential compromise, potential wire fraud |

---

## 2. Attack Context

### 2.1 The Bait
Sarah Chen تلقت إيميل يبدو وكأنه من **"SecureBank National"** البنك اللي بتتعامل معاه شركتها. الإيميل يحتوي على:

From: security@securebänk.com (Punycode: securebxnk.com)
To: sarah.chen@finserve.com
Subject: ⚠️ Urgent: Account Verification Required - Action Needed
Date: Mon, 19 May 2026 08:58:23 +0200
Dear Valued Customer,
We have detected unusual login activity on your business account.
Your account has been temporarily restricted pending verification.
Please verify your account within 24 hours to avoid service disruption.
[VERIFY ACCOUNT NOW] → http://securebänk-verify.com/login
Best regards,
SecureBank Security Team



### 2.2 Technical Deception Indicators

| Indicator | Legitimate | Malicious |
|-----------|-----------|-----------|
| **Domain** | securebank.com | securebänk.com (Punycode: xn--securebnk-0ub.com) |
| **SPF** | PASS | FAIL |
| **DKIM** | VALID | INVALID |
| **DMARC** | ALIGN | MISALIGN |
| **URL** | https://securebank.com | http://securebänk-verify.com |
| **IP** | 203.0.113.10 | 185.220.101.47 (Bulletproof Hosting) |
| **SSL** | Valid EV Cert | Self-Signed / No SSL |

---

## 3. Attack Timeline (Detailed)

| Time | Event | Phase | MITRE Technique |
|------|-------|-------|-----------------|
| **08:58** | Email delivered to inbox | Initial Access | T1566.002 |
| **09:00** | Sarah opens email | Execution | T1204.001 |
| **09:02** | Clicks malicious link | Execution | T1204.001 |
| **09:03** | Redirected to fake login page | Collection | T1567 |
| **09:05** | Enters credentials (username + password) | Credential Access | T1003 |
| **09:06** | Credentials sent to C2 server | Exfiltration | T1041 |
| **09:07** | Redirected to legitimate bank site | Defense Evasion | T1036 |
| **09:15** | Attacker logs in with stolen creds | Lateral Movement | T1021 |
| **09:20** | Downloads account statements | Collection | T1005 |
| **09:30** | Sarah reports suspicious activity to IT | — | — |
| **09:35** | IT forwards to SOC | — | — |
| **09:45** | SOC Analyst confirms phishing | Detection | — |
| **10:00** | IR Playbook activated | Preparation | — |
| **10:15** | Email headers analyzed | Identification | — |
| **10:30** | Domain blocked, endpoint isolated | Containment | T1485 |
| **11:00** | User account suspended | Containment | T1531 |
| **12:00** | Malware scan + credential reset | Eradication | T1205 |
| **13:00** | Systems restored from backup | Recovery | — |
| **14:00** | 72-hour monitoring initiated | Recovery | — |
| **Next Day** | Lessons learned + playbook update | Post-Incident | — |

---

## 4. Forensic Artifacts

### 4.1 Email Artifacts

Message-ID: a1b2c3d4@securebxnk.com
Return-Path: bouncer@securebxnk.com
X-Mailer: PHPMailer 6.5.0 (spoofed)
Received: from mail.securebxnk.com (HELO mx-01.bullet-host.net [185.220.101.47])



### 4.2 Network Indicators
| Type | Value |
|------|-------|
| **Malicious Domain** | securebänk-verify.com |
| **IP Address** | 185.220.101.47 |
| **User-Agent** | Mozilla/5.0 (Windows NT 10.0; Win64; x64) |
| **POST Request** | `POST /api/collect HTTP/1.1` |
| **C2 Beacon** | Every 300 seconds to 185.220.101.47:443 |

### 4.3 Endpoint Artifacts
| Location | Finding |
|----------|---------|
| `C:\Users\sarah.chen\AppData\Local\Temp\` | `update.exe` (MD5: d41d8cd98f00b204e9800998ecf8427e) |
| Browser History | Visited securebänk-verify.com at 09:02 |
| DNS Cache | securebänk-verify.com → 185.220.101.47 |
| Windows Event ID 4624 | Successful logon at 09:15 (suspicious IP) |

---

## 5. IR Response Actions by Phase

### Phase 1: Preparation (Pre-Incident)
- ✅ Employee security awareness training (quarterly)
- ✅ Email gateway with SPF/DKIM/DMARC filtering
- ✅ SIEM monitoring (Splunk) with correlation rules
- ✅ Defined IR team roles and communication channels

### Phase 2: Identification
- 🔍 SIEM alert: "Suspicious Login from Foreign IP"
- 🔍 Email header analysis confirmed spoofing
- 🔍 URL sandbox analysis flagged credential harvester
- 🔍 Severity: **HIGH** (confirmed compromise)

### Phase 3: Containment
- 🛡️ Domain `securebänk-verify.com` blocked at DNS level
- 🛡️ Endpoint `WS-ACCT-042` isolated from network
- 🛡️ User account `sarah.chen` disabled immediately
- 🛡️ Firewall rules updated to block C2 IP

### Phase 4: Eradication
- 🗑️ Full AV scan on affected endpoint
- 🗑️ Browser cache and temp files cleared
- 🗑️ All credentials reset (Sarah + shared accounts)
- 🗑️ Email recalled from all recipients (if applicable)

### Phase 5: Recovery
- 💚 Endpoint reimaged from clean gold image
- 💚 Account re-enabled with MFA enforced
- 💚 72-hour enhanced monitoring activated
- 💚 Business operations verified normal

### Phase 6: Lessons Learned
- 📋 Root Cause: Homograph attack bypassed visual inspection
- 📋 Gap: No punycode detection in email gateway
- 📋 Improvement: Deploy internationalized domain filtering
- 📋 KPI: MTTD 45min | MTTC 90min | MTTR 5hrs ✅

---

## 6. MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique Name | Context |
|--------|-------------|----------------|---------|
| Initial Access | T1566.002 | Spearphishing Link | Malicious email with fake bank link |
| Execution | T1204.001 | User Execution | Sarah clicked the link |
| Persistence | T1136.001 | Local Account Creation | Attacker created backdoor account |
| Credential Access | T1003 | OS Credential Dumping | Harvested login credentials |
| Collection | T1005 | Data from Local System | Downloaded account statements |
| Exfiltration | T1041 | Exfiltration Over C2 | Sent credentials to attacker server |
| Defense Evasion | T1036 | Masquerading | Fake login page mimicked real bank |

---

## 7. Key Takeaways

1. **Homograph attacks** (Punycode) are highly effective against visual inspection
2. **Technical email authentication** (SPF/DKIM/DMARC) must be enforced, not just monitored
3. **User reporting** was the critical detection mechanism (not automated tools)
4. **Rapid containment** (90 min) prevented financial loss
5. **MFA would have prevented** the account takeover even with stolen password

---

*Scenario developed for: Incident Response Course - 2026*  
*Team: Mohamed Abbasy | Mohamed Hesham | Marvel Ghobrial | Omar *  
*Supervisor: Eng. Mohamed*

