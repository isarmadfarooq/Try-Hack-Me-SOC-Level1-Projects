# 🔐 SOC Incident Analysis Report — Phishing Campaign

<p align="center">
  <img src="https://img.shields.io/badge/Platform-TryHackMe-red?style=for-the-badge&logo=tryhackme&logoColor=white" />
  <img src="https://img.shields.io/badge/Category-Phishing%20Analysis-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Team-Blue%20Team-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/SIEM-Splunk-green?style=for-the-badge&logo=splunk&logoColor=white" />
  <img src="https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-black?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge" />
</p>

<p align="center">
  <b>A hands-on SOC simulation investigating a real-world phishing campaign — built as part of an MS Cybersecurity portfolio.</b>
</p>

---

## 👨‍💻 Analyst Profile

| Field | Details |
|---|---|
| **Name** | Sarmad Farooq |
| **Role** | MS Cybersecurity Student · Aspiring SOC Analyst / Security Engineer |
| **Focus Areas** | Network Security · Threat Detection · Incident Response |
| **LinkedIn** | [linkedin.com/in/sarmad-farooq-cybersecurity-engineer](https://www.linkedin.com/in/sarmad-farooq-cybersecurity-engineer/) |
| **GitHub** | [github.com/isarmadfarooq](https://github.com/isarmadfarooq) |

---

## 📄 Report Overview

| Attribute | Value |
|---|---|
| **Report Reference** | SOC-SIM-2026-001 |
| **Scenario** | Introduction to Phishing — TryHackMe SOC Simulator |
| **Classification** | CONFIDENTIAL — For Training Purposes |
| **Severity** | MEDIUM — Blocked at Perimeter |
| **MITRE ATT&CK Tactic** | Initial Access — T1566 (Phishing) |
| **Alerts Investigated** | 4 (Alerts 8814, 8815, 8816, 8817) |
| **True Positives** | 3 |
| **False Positives** | 1 |
| **Outcome** | All threats contained · All alerts closed |

---

## 🎯 Objective

This project simulates a real-world **Security Operations Center (SOC)** environment using the TryHackMe SOC Simulator. As the analyst, I triaged, investigated, and closed a queue of phishing-related security alerts — applying industry-standard SOC methodology from initial alert assignment through to incident report closure.

**Core skills demonstrated:**

| # | Skill |
|---|---|
| 1 | 🔍 Alert triage and prioritisation within a SIEM alert queue |
| 2 | 📧 Email header and sender domain analysis |
| 3 | 🔗 Malicious URL identification — shortened URLs and typosquatted domains |
| 4 | 🧩 Multi-source log correlation across email, firewall, and proxy datasources |
| 5 | 🧠 True Positive / False Positive classification with documented justification |
| 6 | 🌐 Threat intelligence enrichment via AbuseIPDB |
| 7 | 🗺️ MITRE ATT&CK technique mapping |
| 8 | 📝 Professional incident report documentation and formal case closure |

---

## 🚨 Alerts Investigated

### 🔵 Alert 8814 — Inbound Email: Suspicious External Link

| Field | Detail |
|---|---|
| **Timestamp** | 03/18/2026 16:00:46 |
| **Sender** | onboarding@hrconnex.thm |
| **Recipient** | j.garcia@thetrydaily.thm |
| **Subject** | Action Required: Finalize Your Onboarding Profile |
| **Embedded URL** | https://hrconnex.thm/onboarding/15400654060/j.garcia |
| **Data Source** | Email |
| **Verdict** | ✅ FALSE POSITIVE |
| **Justification** | SIEM correlation confirmed `hrconnex.thm` as a pre-approved third-party HR onboarding partner. A prior internal HR email had notified all staff of communications from this domain. No malicious activity detected in firewall or proxy logs. |

---

### 🔴 Alert 8815 — Inbound Email: Amazon Brand Impersonation

| Field | Detail |
|---|---|
| **Timestamp** | 11/20/2025 20:03:09 |
| **Sender** | urgents@amazon.biz |
| **Recipient** | h.harris@thetrydaily.thm |
| **Subject** | Your Amazon Package Couldn't Be Delivered — Action Required |
| **Malicious URL** | http://bit.ly/3sHkX3da12340 |
| **Data Source** | Email / Network Security |
| **Verdict** | 🔴 TRUE POSITIVE |
| **Justification** | Spoofed `.biz` domain impersonating Amazon. Bit.ly shortener used to obfuscate malicious destination. SIEM confirmed user click-through event. Connection blocked by perimeter firewall — correlated with Alert 8816. |

---

### 🔴 Alert 8816 — Blacklisted URL Blocked by Firewall

| Field | Detail |
|---|---|
| **Timestamp** | 11/20/2025 20:04:23 |
| **Source IP** | 10.20.2.17 (h.harris endpoint) |
| **Destination IP** | 67.199.248.11 |
| **Destination Port** | 80 (HTTP) |
| **Blocked URL** | http://bit.ly/3sHkX3da12340 |
| **Firewall Action** | BLOCKED — Rule: Blocked Websites |
| **Protocol** | TCP |
| **Data Source** | Firewall / Network Security |
| **Verdict** | 🔴 TRUE POSITIVE |
| **Justification** | Perimeter firewall block confirmed for blacklisted URL. Triggered 74 seconds after Alert 8815 email delivery — same user, same URL. Represents the network-layer evidence of the Alert 8815 click-through. Attack chain fully contained at perimeter. |

> 💡 **Attack Chain:** Alerts 8815 and 8816 are correlated as a single two-event attack chain — phishing email delivery → user click-through → firewall block. This cross-datasource correlation (email + firewall) is a key analytical finding in this investigation.

---

### 🔴 Alert 8817 — Inbound Email: Microsoft Account Security Spoof

| Field | Detail |
|---|---|
| **Timestamp** | 11/20/2025 20:05:27 |
| **Sender** | no-reply@m1crosoftsupport.co |
| **Recipient** | c.allen@thetrydaily.thm |
| **Subject** | Unusual Sign-In Activity on Your Microsoft Account |
| **Malicious URL** | https://m1crosoftsupport.co/login |
| **IP Referenced in Email** | 102.89.222.143 — AbuseIPDB: Confirmed Malicious |
| **Data Source** | Email & Messaging |
| **Verdict** | 🔴 TRUE POSITIVE |
| **Justification** | Typosquatted Microsoft domain — digit `1` replacing letter `i` in `microsoft`. Credential-harvesting `/login` path. AbuseIPDB confirmed the referenced IP as malicious. Classic account-security fear-based social engineering pretext with fake sign-in alert from Nigeria. |

---

## 📊 Key Findings Summary

```
╔══════════════════════════════════════════════════════════════╗
║              INVESTIGATION RESULTS SUMMARY                   ║
╠══════════════════════════════════════════════════════════════╣
║  Total Alerts Investigated  :  4                             ║
║  True Positives  (TP)       :  3  (Alerts 8815, 8816, 8817) ║
║  False Positives (FP)       :  1  (Alert 8814)              ║
║  Escalations Required       :  0                             ║
║  Data Exfiltration Detected :  None                          ║
║  Lateral Movement Detected  :  None                          ║
║  Attack Chain Disrupted     :  Yes — perimeter FW block      ║
╚══════════════════════════════════════════════════════════════╝
```

---

## 🔎 Indicators of Compromise (IOC Register)

| IOC Type | Indicator | Alert | Verdict |
|---|---|---|---|
| Domain | `hrconnex.thm` | 8814 | ✅ Legitimate |
| Email Sender | `urgents@amazon.biz` | 8815 | 🔴 Malicious |
| URL | `http://bit.ly/3sHkX3da12340` | 8815 / 8816 | 🔴 Malicious |
| IP Address | `67.199.248.11` | 8816 | 🔴 Blacklisted |
| Email Sender | `no-reply@m1crosoftsupport.co` | 8817 | 🔴 Malicious |
| Domain | `m1crosoftsupport.co` (typosquat) | 8817 | 🔴 Malicious |
| IP Address | `102.89.222.143` (AbuseIPDB) | 8817 | 🔴 Malicious |

---

## 🧬 MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique Name | Observed Behaviour |
|---|---|---|---|
| Initial Access | T1566.001 | Spearphishing Link | Malicious URLs embedded directly in phishing emails |
| Initial Access | T1566.002 | Spearphishing via Link | Bit.ly shortener redirecting to malicious destination |
| Reconnaissance | T1598.003 | Phishing for Information | Brand impersonation of Amazon and Microsoft |
| Resource Development | T1583.001 | Acquire Infrastructure | Typosquatted domain `m1crosoftsupport.co` registered by threat actor |
| Defense Evasion | T1036.005 | Masquerading | Spoofed sender domains (`amazon.biz`, `m1crosoftsupport.co`) |

---

## ⚙️ Tools & Technologies

| Tool / Platform | Purpose in Investigation |
|---|---|
| **Splunk SIEM** (Simulated) | Alert triage, SPL log queries, multi-datasource correlation |
| **Email Analysis** | Header inspection, sender domain verification, URL extraction |
| **AbuseIPDB** | IP reputation lookup, abuse scoring, ASN and geolocation |
| **Firewall / Proxy Logs** | Outbound connection monitoring, blacklist rule enforcement |
| **MITRE ATT&CK Framework** | TTP classification and standardised technique mapping |
| **SOC Workflow** | Alert assignment, case creation, triage, classification, formal closure |

---

## 🛡️ Recommendations

### Immediate — 0 to 24 Hours

- Block all malicious IOCs (`67.199.248.11`, `amazon.biz`, `m1crosoftsupport.co`, `102.89.222.143`) at firewall, email gateway, and DNS
- Notify affected users `h.harris` and `c.allen` — confirm no credentials were entered on any external site
- Quarantine and purge phishing emails from all recipient mailboxes organisation-wide

### Short-Term — 1 to 7 Days

- Deploy targeted phishing awareness training covering URL shortener risks, typosquatting, and urgency-based social engineering
- Whitelist `hrconnex.thm` in email security gateway to reduce future False Positive alert volume
- Add TLD anomaly detection rules to email gateway to flag `.biz` and `.co` domains impersonating `.com`

### Long-Term — 30+ Days

- Integrate AbuseIPDB and VirusTotal APIs into SIEM for automated IOC enrichment on all email and firewall alerts
- Deploy URL sandboxing (detonation chamber) to resolve and inspect all inbound email links before delivery to end-users
- Enforce DMARC, DKIM, and SPF email authentication policies to reduce brand spoofing risk
- Conduct quarterly phishing simulation exercises to measure and improve employee security posture

---

## 📚 Learning Outcomes

**Alert Fatigue Awareness**
Correctly identified Alert 8814 as a False Positive through SIEM correlation rather than reactive classification. Automated rules frequently flag legitimate external links — analysts must validate against organisational context before escalating.

**Attack Chain Correlation**
Linked Alerts 8815 and 8816 as a single correlated attack chain spanning two separate datasources (email and firewall). Effective SOC analysis requires looking beyond individual alert boundaries to identify multi-event threat sequences.

**Social Engineering Recognition**
Identified urgency, fear, and brand impersonation tactics consistently applied across all three True Positive phishing emails — patterns present in the majority of real-world phishing campaigns.

**Threat Intelligence Integration**
Used AbuseIPDB to corroborate True Positive classification in Alert 8817 through external IP reputation data — demonstrating the value of cross-referencing internal SIEM findings with external intelligence sources.

**Structured Incident Documentation**
Produced formal incident reports for all four alerts including classification, justification, response actions, and formal case closure — aligned with Tier 1 SOC analyst documentation standards.

---

## 📁 Repository Contents

```
📦 SOC-Simulation-Phishing-Report
 ┣ 📄 README.md                            ← You are here
 ┗ 📄 SOC_Simulation_Report_Advanced.docx  ← Full technical report (11 sections, 30 screenshots)
```

> **Full Report:** [`SOC_Simulation_Report_Advanced.docx`](./SOC_Simulation_Report_Advanced.docx)
>
> The complete report contains 11 sections including per-alert investigation deep-dives, an IOC register, MITRE ATT&CK mapping, analyst skills matrix, and tiered remediation recommendations — with all 30 annotated investigation screenshots preserved.

---

<p align="center">
  <i>Completed as part of MS Cybersecurity studies and personal SOC skills development portfolio.</i><br/>
  <i>All simulation data is from the TryHackMe SOC Simulator platform — for educational purposes only.</i>
  <br/><br/>
  <b>Sarmad Farooq</b> · MS Cybersecurity · Aspiring SOC Analyst / Security Engineer
  <br/>
  <a href="https://www.linkedin.com/in/sarmad-farooq-cybersecurity-engineer/">LinkedIn</a> &nbsp;·&nbsp; <a href="https://github.com/isarmadfarooq">GitHub</a>
</p>
