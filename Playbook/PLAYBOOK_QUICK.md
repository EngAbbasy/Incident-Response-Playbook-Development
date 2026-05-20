# 🛡️ IR Playbook — Phishing Attack (Quick Response)

&gt; **Version:** 1.0 | **Date:** 2026-05-20  
&gt; **Team:** Mohamed Abbasy | Mohamed Hesham | Marvel Ghobrial  
&gt; **Scenario:** Bank Impersonation Phishing

---

## ⚡ The 5-Minute Summary

| What Happened | Employee clicked fake bank link, credentials stolen |
|---------------|---------------------------------------------------|
| **Trigger** | User report / SIEM alert / Email gateway flag |
| **Severity** | HIGH (confirmed credential compromise) |
| **Goal** | Contain in 90 min, recover in 5 hours |

---

## 🚨 Immediate Actions (First 15 Minutes)

PRESERVE  → Save original email (.eml file)
ANALYZE   → Check URL on VirusTotal + URLScan
CLASSIFY  → LOW / MEDIUM / HIGH / CRITICAL
ALERT     → Notify IR Lead if HIGH or above
CONTAIN   → Block domain + isolate endpoint


---

## 📋 6-Phase Response (Checklist)

### Phase 1: PREPARATION (Done Before)
- [ ] Email gateway rules active (SPF/DKIM/DMARC)
- [ ] IR team contact list updated
- [ ] Isolation scripts ready

### Phase 2: IDENTIFICATION (0-30 min)
- [ ] Capture email headers
- [ ] Analyze URL in sandbox
- [ ] Interview user (what was clicked/entered)
- [ ] Check SIEM for suspicious logins
- [ ] **Classify severity**

### Phase 3: CONTAINMENT (30-90 min)
- [ ] Block malicious domain at DNS
- [ ] Block C2 IP on firewall
- [ ] Isolate affected endpoint
- [ ] Disable compromised account
- [ ] Revoke active sessions
- [ ] Notify bank (if financial account involved)
- [ ] Hunt for other affected users

### Phase 4: ERADICATION (90 min - 2.5 hrs)
- [ ] Full AV/EDR scan
- [ ] Clear browser cache/cookies
- [ ] Check for persistence (tasks, registry)
- [ ] Reset all passwords for user
- [ ] Reset shared accounts if exposed
- [ ] Verify clean (second scan)

### Phase 5: RECOVERY (2.5 - 5 hrs)
- [ ] Reimage endpoint OR restore from clean backup
- [ ] Re-enable account with MFA
- [ ] Verify bank access with new credentials
- [ ] 72-hour enhanced monitoring
- [ ] Confirm business operations normal

### Phase 6: LESSONS LEARNED (Next Day)
- [ ] Document timeline
- [ ] Calculate MTTD / MTTC / MTTR
- [ ] Update playbook
- [ ] Update threat intel (IOCs)
- [ ] Team debrief

---

## 🎯 Decision Tree
Alert Received
│
▼
┌─────────────────┐
│ User report or  │
│ automated?      │
└─────────────────┘
│
├──► User report ──► Triage 15 min
│                      │
│                      ▼
│              ┌─────────────┐
│              │ Confirmed   │
│              │ malicious?  │
│              └─────────────┘
│                      │
│           YES ◄─────┼─────► NO → Close
│           │
│           ▼
└──► Auto alert ──► Classify immediately
│
▼
┌─────────────────────┐
│ HIGH / CRITICAL?    │
└─────────────────────┘
│
YES ◄────┼────► NO → Standard response
│
▼
Activate full IR team
│
▼
┌─────────────────────┐
│ Credential          │
│ compromise?         │
└─────────────────────┘
│
YES ◄┼────► NO → Contain email only
│
▼
Notify bank + legal
Execute full playbook



---

## 👥 Who Does What

| Role | Person | Responsibilities |
|------|--------|----------------|
| **IR Lead** | Mohamed Abbasy | Commander, decisions, external comms |
| **SOC Analyst** | Mohamed Hesham | Detection, analysis, containment |
| **Forensics** | Marvel Ghobrial | Evidence, malware analysis, recovery |

---

## 📞 Quick Contacts

| Role | Contact |
|------|---------|
| IR Lead | Mohamed Abbasy — @mohamed.abbasy |
| SOC | Mohamed Hesham — @mohamed.hesham |
| Forensics | Marvel Ghobrial — @marvel.ghobrial |
| Bank Security | [Bank SOC Number] |
| Legal | [Legal Contact] |

---

## 🏆 Targets (KPIs)

| Metric | Target | This Incident |
|--------|--------|---------------|
| **MTTD** (Detect) | < 60 min | 45 min ✅ |
| **MTTC** (Contain) | < 2 hrs | 90 min ✅ |
| **MTTR** (Recover) | < 8 hrs | 5 hrs ✅ |

---

## 🔍 Key IOCs (This Incident)
Domain:  securebänk-verify.com
xn--securebnk-0ub.com (Punycode)
IP:      185.220.101.47
URL:     http://securebänk-verify.com/login
Hash:    d41d8cd98f00b204e9800998ecf8427e (update.exe)


---

## 📝 Post-Incident (Next Day)

- [ ] Incident report completed
- [ ] Playbook updated (v1.1)
- [ ] Threat intel feeds updated
- [ ] Team debrief done
- [ ] Executive briefing sent

---

> **Remember:** Speed beats perfection in incident response. Contain first, analyze later.

**END OF PLAYBOOK**
