\# 🔐 SOC Incident Analysis Report — Phishing Campaign



<p align="center">

&#x20; <img src="https://img.shields.io/badge/Platform-TryHackMe-red?style=for-the-badge\&logo=tryhackme\&logoColor=white" />

&#x20; <img src="https://img.shields.io/badge/Category-Phishing%20Analysis-orange?style=for-the-badge" />

&#x20; <img src="https://img.shields.io/badge/Team-Blue%20Team-blue?style=for-the-badge" />

&#x20; <img src="https://img.shields.io/badge/SIEM-Splunk-green?style=for-the-badge\&logo=splunk\&logoColor=white" />

&#x20; <img src="https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-black?style=for-the-badge" />

&#x20; <img src="https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge" />

</p>



\---



\## 👨‍💻 Analyst



| Field | Details |

|---|---|

| \*\*Name\*\* | Sarmad Farooq |

| \*\*Role\*\* | MS Cybersecurity Student · Aspiring SOC Analyst / Security Engineer |

| \*\*Focus Areas\*\* | Network Security · Threat Detection · Incident Response |

| \*\*LinkedIn\*\* | \[linkedin.com/in/sarmad-farooq-cybersecurity-engineer](https://www.linkedin.com/in/sarmad-farooq-cybersecurity-engineer/) |

| \*\*GitHub\*\* | \[github.com/isarmadfarooq](https://github.com/isarmadfarooq) |



\---



\## 📄 Report Overview



| Attribute | Value |

|---|---|

| \*\*Report Reference\*\* | SOC-SIM-2026-001 |

| \*\*Scenario\*\* | Introduction to Phishing — TryHackMe SOC Simulator |

| \*\*Classification\*\* | CONFIDENTIAL — For Training Purposes |

| \*\*Severity\*\* | MEDIUM — Blocked at Perimeter |

| \*\*MITRE ATT\&CK Tactic\*\* | Initial Access — T1566 (Phishing) |

| \*\*Alerts Investigated\*\* | 4 (Alerts 8814, 8815, 8816, 8817) |

| \*\*True Positives\*\* | 3 |

| \*\*False Positives\*\* | 1 |

| \*\*Outcome\*\* | All threats contained · All alerts closed |



\---



\## 🎯 Objective



This project simulates a real-world \*\*Security Operations Center (SOC)\*\* environment using the TryHackMe SOC Simulator. The analyst triages, investigates, and closes a queue of phishing-related alerts, applying industry-standard SOC methodology throughout.



\*\*Core skills demonstrated:\*\*



\- 🔍 Alert triage and prioritisation within a SIEM alert queue

\- 📧 Email header and sender domain analysis

\- 🔗 Malicious URL identification (shortened URLs, typosquatted domains)

\- 🧩 Multi-source log correlation (email + firewall + proxy)

\- 🧠 True Positive / False Positive classification with documented justification

\- 🌐 Threat intelligence enrichment via AbuseIPDB

\- 🗺️ MITRE ATT\&CK technique mapping

\- 📝 Professional incident report documentation and case closure



\---



\## 🚨 Alerts Investigated



\### Alert 8814 — Inbound Email Containing Suspicious External Link

| Field | Detail |

|---|---|

| \*\*Sender\*\* | onboarding@hrconnex.thm |

| \*\*Recipient\*\* | j.garcia@thetrydaily.thm |

| \*\*Subject\*\* | Action Required: Finalize Your Onboarding Profile |

| \*\*Verdict\*\* | ✅ \*\*FALSE POSITIVE\*\* |

| \*\*Justification\*\* | SIEM correlation confirmed hrconnex.thm as a pre-approved third-party HR onboarding partner. Prior internal HR email notified all staff of communications from this domain. No malicious activity detected. |



\---



\### Alert 8815 — Inbound Email Containing Suspicious External Link (Amazon Spoof)

| Field | Detail |

|---|---|

| \*\*Sender\*\* | urgents@amazon.biz |

| \*\*Recipient\*\* | h.harris@thetrydaily.thm |

| \*\*Subject\*\* | Your Amazon Package Couldn't Be Delivered — Action Required |

| \*\*Malicious URL\*\* | http://bit.ly/3sHkX3da12340 |

| \*\*Verdict\*\* | 🔴 \*\*TRUE POSITIVE\*\* |

| \*\*Justification\*\* | Spoofed `.biz` domain impersonating Amazon. Bit.ly URL shortener used to obfuscate malicious destination. SIEM confirmed user click-through; connection blocked by firewall (correlated with Alert 8816). |



\---



\### Alert 8816 — Access to Blacklisted External URL Blocked by Firewall

| Field | Detail |

|---|---|

| \*\*Source IP\*\* | 10.20.2.17 (h.harris endpoint) |

| \*\*Destination IP\*\* | 67.199.248.11 |

| \*\*Destination Port\*\* | 80 (HTTP) |

| \*\*Blocked URL\*\* | http://bit.ly/3sHkX3da12340 |

| \*\*Firewall Action\*\* | BLOCKED — Rule: Blocked Websites |

| \*\*Verdict\*\* | 🔴 \*\*TRUE POSITIVE\*\* |

| \*\*Justification\*\* | Firewall perimeter block confirmed for blacklisted URL. Direct correlation to Alert 8815 — same user, same URL, 74 seconds after email delivery. Attack chain fully contained at perimeter. |



\---



\### Alert 8817 — Inbound Email Containing Suspicious External Link (Microsoft Spoof)

| Field | Detail |

|---|---|

| \*\*Sender\*\* | no-reply@m1crosoftsupport.co |

| \*\*Recipient\*\* | c.allen@thetrydaily.thm |

| \*\*Subject\*\* | Unusual Sign-In Activity on Your Microsoft Account |

| \*\*Malicious URL\*\* | https://m1crosoftsupport.co/login |

| \*\*Spoofed IP in Email\*\* | 102.89.222.143 (AbuseIPDB: Confirmed Malicious) |

| \*\*Verdict\*\* | 🔴 \*\*TRUE POSITIVE\*\* |

| \*\*Justification\*\* | Typosquatted Microsoft domain (digit "1" replacing letter "i"). Credential-harvesting `/login` URL. AbuseIPDB confirmed malicious source IP. Classic account-security fear-based social engineering pretext. |



\---



\## 🧬 MITRE ATT\&CK Mapping



| Tactic | Technique ID | Description |

|---|---|---|

| Initial Access | T1566.001 | Spearphishing Link — embedded malicious URLs |

| Initial Access | T1566.002 | Spearphishing via Link — bit.ly URL shortener redirect |

| Reconnaissance | T1598.003 | Phishing for Information — brand impersonation |

| Resource Development | T1583.001 | Acquire Infrastructure — typosquatted domains |

| Defense Evasion | T1036.005 | Masquerading — spoofed sender domains |



\---



\## 🔎 Indicators of Compromise (IOCs)



| IOC Type | Indicator | Alert | Verdict |

|---|---|---|---|

| Domain | hrconnex.thm | 8814 | ✅ Legitimate |

| Email Sender | urgents@amazon.biz | 8815 | 🔴 Malicious |

| URL | http://bit.ly/3sHkX3da12340 | 8815 / 8816 | 🔴 Malicious |

| IP Address | 67.199.248.11 | 8816 | 🔴 Blacklisted |

| Email Sender | no-reply@m1crosoftsupport.co | 8817 | 🔴 Malicious |

| Domain | m1crosoftsupport.co (typosquat) | 8817 | 🔴 Malicious |

| IP Address (AbuseIPDB) | 102.89.222.143 | 8817 | 🔴 Malicious |



\---



\## ⚙️ Tools \& Technologies



| Tool | Purpose |

|---|---|

| \*\*Splunk SIEM\*\* (Simulated) | Alert triage, log search, multi-source correlation |

| \*\*Email Analysis\*\* | Header inspection, sender domain verification, link extraction |

| \*\*AbuseIPDB\*\* | IP reputation lookup, abuse score, geolocation |

| \*\*Firewall / Proxy Logs\*\* | Outbound connection monitoring, blacklist enforcement |

| \*\*MITRE ATT\&CK Framework\*\* | TTP classification and technique mapping |

| \*\*SOC Investigation Workflow\*\* | Alert assignment, case creation, triage, classification, closure |



\---



\## 📁 Repository Contents



```

📦 SOC-Simulation-Phishing-Report

&#x20;┣ 📄 README.md                          ← You are here

&#x20;┗ 📄 SOC\_Simulation\_Report\_Advanced.docx ← Full technical incident report

```



\---



\## 📊 Key Findings Summary



```

Total Alerts Investigated  : 4

True Positives (TP)        : 3  (Alerts 8815, 8816, 8817)

False Positives (FP)       : 1  (Alert 8814)

Escalations Required       : 0  (all threats contained at perimeter)

Data Exfiltration Detected : None

Lateral Movement Detected  : None

Attack Chain Disrupted     : ✅ Yes — perimeter firewall block confirmed

```



> \*\*Critical finding:\*\* Alerts 8815 and 8816 represent a correlated two-event attack chain — a phishing email induced a user click-through (8815) which was blocked by the perimeter firewall exactly 74 seconds later (8816). This correlation demonstrates the importance of cross-datasource alert analysis in SOC operations.



\---



\## 📚 Learning Outcomes



Through this simulation I developed and demonstrated:



\- \*\*Alert fatigue awareness\*\* — correctly identifying a False Positive (8814) through SIEM correlation rather than reactive classification

\- \*\*Attack chain analysis\*\* — linking two separate alerts (8815 + 8816) as a single correlated threat event across email and firewall data sources

\- \*\*Social engineering pattern recognition\*\* — identifying urgency, fear, and brand impersonation tactics across all three True Positive phishing emails

\- \*\*Threat intelligence integration\*\* — using AbuseIPDB to corroborate TP classification with external reputation data

\- \*\*Structured documentation\*\* — producing professional incident reports aligned with industry SOC standards for every alert investigated



\---



\## 🛡️ Recommendations



\### Immediate (0–24 Hours)

\- Block all malicious IOCs at firewall, email gateway, and DNS

\- Notify affected users (h.harris, c.allen) and confirm no credentials were submitted

\- Quarantine and purge phishing emails from all mailboxes



\### Short-Term (1–7 Days)

\- Deploy phishing awareness training focusing on URL shorteners, typosquatting, and urgency tactics

\- Whitelist `hrconnex.thm` to reduce future False Positive volume

\- Implement TLD anomaly detection rules in email gateway (`.biz` / `.co` vs `.com`)



\### Long-Term (30+ Days)

\- Integrate AbuseIPDB and VirusTotal APIs for automated IOC enrichment in SIEM

\- Deploy URL sandboxing / detonation chamber for all inbound email links

\- Enforce DMARC, DKIM, and SPF policies to reduce brand spoofing risk

\- Conduct regular phishing simulation exercises to track employee security posture



\---



\## 📜 Report Details



> \*\*Full Report:\*\* \[`SOC\_Simulation\_Report\_Advanced.docx`](./SOC\_Simulation\_Report\_Advanced.docx)

>

> The full report (11 sections, 30 annotated screenshots, IOC register, MITRE ATT\&CK mapping, and tiered remediation recommendations) is available in the `.docx` file above.



\---



<p align="center">

&#x20; <i>This project was completed as part of my MS Cybersecurity studies and personal SOC skills development portfolio.</i><br/>

&#x20; <i>All simulation data is from the TryHackMe SOC Simulator platform and is for educational purposes only.</i>

</p>



<p align="center">

&#x20; <b>Sarmad Farooq</b> · MS Cybersecurity · Aspiring SOC Analyst / Security Engineer<br/>

&#x20; <a href="https://www.linkedin.com/in/sarmad-farooq-cybersecurity-engineer/">LinkedIn</a> · <a href="https://github.com/isarmadfarooq">GitHub</a>

</p>

