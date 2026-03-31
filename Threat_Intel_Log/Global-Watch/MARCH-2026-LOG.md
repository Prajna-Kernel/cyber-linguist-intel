# 🛡️ Threat Intelligence Log — March 2026

> Active monitoring of cross-border cyber operations and state-sponsored activity.
> Focus: Non-regional incidents, wiper attacks, and AI-assisted threat tooling.
> Language access: English

---

### Incident #001 — March 28, 2026

**Target:** Kash Patel (FBI Director) — Personal Email | Stryker Corporation (Fortune 500, Medical Devices)

**Sector:** Law Enforcement / Healthcare & Defence Manufacturing

**Threat Actor:** Handala Hack Team

**Origin:** Iran — Ministry of Intelligence and Security (MOIS)
Also tracked as: Banished Kitten | Cobalt Mystique | Red Sandstorm | Void Manticore

**Source:** The Hacker News — March 28, 2026 | Reuters | Flashpoint | Check Point Research | Unit 42 (Palo Alto Networks)

**Attack Type:** Hack-and-Leak (FBI Director) + Destructive Wiper Attack (Stryker)

**Labels:** Hacktivism | State-Sponsored | Wiper Malware | Geopolitical | Critical Infrastructure

---

### Analysis
Handala Hack Team — a MOIS-affiliated group operating under a hacktivist persona — executed two simultaneous operations on March 28, 2026.

**Operation 1 — FBI Director Email Breach:**
Handala successfully compromised the personal email account of FBI Director Kash Patel and leaked photos and documents publicly. The FBI confirmed the breach, noting the data was historical and contained no government information. The breach was likely enabled through phishing or compromised credentials obtained via infostealer malware.

**Operation 2 — Stryker Wiper Attack:**
Handala deployed wiper malware against Stryker Corporation, deleting large volumes of company data and wiping thousands of employee devices. This marks the first confirmed destructive wiper attack against a U.S. Fortune 500 company. Initial access was achieved through Microsoft Intune via compromised administrative credentials. Stryker has since contained the incident and dismantled the persistence mechanisms installed.

---

## Key Technical Indicators:
- **Initial Access:** Phishing / Compromised Microsoft Intune admin credentials (infostealer-sourced)

- **Lateral Movement:** RDP

- **Payload:** Handala Wiper + Handala PowerShell Wiper (deployed via Group Policy logon scripts)

- **Evasion:** VeraCrypt (legitimate disk encryption used to complicate recovery)

- **C2:** Telegram (used to blend malicious traffic with legitimate platform activity)

- *Malware also capable of recording audio and screen during active Zoom sessions*

---

## Strategic Context
These attacks were retaliatory — triggered by a U.S. court-authorised seizure of four MOIS-operated domains (handala-hack[.]to, justicehomeland[.]org, karmabelow80[.]org, handala-redwanted[.]to). The U.S. DOJ stated these domains were used for psychological operations, data leaks, and targeting of Iranian dissidents and Israeli government personnel.

The Stryker attack represents a documented shift: Handala is moving from espionage and data theft toward active destruction of Western infrastructure. Flashpoint has flagged this as a dangerous evolution in supply chain threats — disrupting a major medical device supplier creates cascading risk across the entire healthcare ecosystem.

*The U.S. government is offering a $10 million reward for information on Handala members.*

---

### Incident #002 — March 31, 2026 (Global-Watch)

**​Target:** Citrix NetScaler ADC & Gateway

**Sector:** Global Enterprise Infrastructure / Remote Access

​**Threat Actor:** Unidentified (Multiple groups observed)

**​Origin:** Global (Distributed scanning and exploitation)

​**Source:** BleepingComputer | CISA KEV | Citrix Security Bulletin

**​Attack Type:** Memory Overread / Information Disclosure (CVE-2026-3055)

**​Labels:** Active-Exploitation | Critical-Infrastructure | Memory-Bleed | 0-Day

## ​Analysis
​As of March 30-31, 2026, security researchers have confirmed that CVE-2026-3055 is now under active exploitation. What began as automated reconnaissance has shifted into targeted attacks.
​The "Bleed" Mechanism: Attackers are using a specific buffer overread to "bleed" the system's memory. This allows for the theft of session cookies, which lets hackers bypass Multi-Factor Authentication (MFA) and enter corporate networks as legitimate users.

**​Mass Exploitation:** BleepingComputer reports that thousands of instances remain unpatched. Since this is an edge-of-network device (the "front door"), a single successful exploit grants total internal access.

**​Urgency:** CISA has added this to the KEV catalog, and Citrix has issued an "Emergency Patch" advisory. Organizations are urged to rotate all session secrets and certificates after patching.

---

### Incident #003 — March 31, 2026 (Global-Watch)

**Target:** QualDerm Partners (Dermatology Management Services)

**Sector:** Healthcare / PHI (Protected Health Information)

**Threat Actor:** Unidentified Third Party (Attribution Pending)

**Origin:** Unknown (Likely Cyber-Criminal/Extortion Group)

**Source:** HHS Breach Portal | VCDB Issue #3104 | SecurityWeek

**Attack Type:** IT Hacking / Data Exfiltration (HHS Category)

**Labels:** Data-Breach | Healthcare-Security | PHI-Exfiltration | 3M-Impact

---

## Analysis
QualDerm Partners, which manages over 150 practices across 17 US states, officially reported a massive breach affecting 3,117,874 individuals.

**Timeline of Disclosure:** Although the unauthorized access occurred between December 23–24, 2025, the full scale of the 3.1 million victim count was only confirmed and added to the HHS (Department of Health and Human Services) portal in late March 2026. This delay is due to the mandatory 60-day reporting window and the time required for deep forensic verification.

**The "Intrusive" Window:** Attackers had access for only 48 hours but successfully targeted and removed data from a limited number of sensitive internal systems.

**Stolen Data Points:** The exfiltrated information includes patient names, DOB, medical record numbers, diagnoses, treatment plans, health insurance details, and in some cases, Social Security numbers and Driver’s License numbers.

---

### Incident #004 — March 31, 2026 (Global-Watch)

**Target:** Axios NPM Package (Supply Chain)

**Sector:** Software Development / Global JS Ecosystem

**Threat Actor:** Unidentified (Malicious maintainer injection)

**Source:** BleepingComputer | The Hacker News | Snyk Security

**Attack Type:** Supply Chain Hijack / Remote Access Trojan (RAT)

**Labels:** Critical-Severity | Supply-Chain | Active-Exploitation | RAT-Dropper

---

## Technical Analysis:
A high-criticality supply chain compromise affected the official Axios package. Version 1.14.1 and 0.30.4 were injected with a "phantom dependency" (plain-crypto-js@4.2.1). Upon installation, this dependency executes a post-install script that detects the host OS (Windows/Mac/Linux) and drops a tailored Remote Access Trojan (RAT).

**TTPs:** The RAT performs automated reconnaissance, specifically targeting .env files, browser cookies, and local ~/.ssh directories to facilitate cloud account takeovers.

**Mitigation:** Immediate audit of package-lock.json. Force-downgrade to verified version 1.7.x or upgrade to the emergency patch 1.14.2.

---

### Incident #005 — March 31, 2026 (Global-Watch)

**Target:** Google Vertex AI (Cloud Platform)

**Sector:** Cloud Infrastructure / Artificial Intelligence (AI)

**Threat Actor:** Security Researchers (Vulnerability Disclosure)

**Source:** The Hacker News | Orca Security

**Attack Type:** Cloud-to-Cloud Vulnerability / Privilege Escalation

**Labels:** Cloud-Security | AI-Infrastructure | Google-Cloud | Vertex-AI

---

## Technical Analysis:
Analysis of CVE-2026-2473, a flaw in Google Vertex AI's bucket-naming logic. By "squatting" on predictable Cloud Storage bucket names, an attacker could intercept data intended for AI model training or execute cross-tenant privilege escalation.

**Significance:** This represents a new class of "AI-Native" vulnerabilities where the complexity of the ML pipeline creates traditional IAM (Identity and Access Management) blind spots.

**Impact:** Potentially allowed unauthorized access to proprietary machine learning models and sensitive training datasets.

---

### Incident #006 — March 31, 2026 (Global-Watch)

**Target:** Town of Apex, North Carolina (Local Government)

**Sector:** Public Administration / NAICS 92

**Source:** WRAL News | VCDB Issue #3107

**Attack Type:** Ransomware Recovery (July 2024 Attack / March 2026 Closure)

**Labels:** Municipal-Security | Legal-Precedent | Data-Recovery | FBI-Involved

---

## Technical Analysis:
Conclusion of the Town of Apex recovery operation (originally attacked July 2024). This case provides a landmark legal template for local governments. By securing a Temporary Restraining Order (TRO) against a US-based cloud provider (Bublup, Inc.), the town **successfully recovered** exfiltrated data without paying a ransom.

**Key Finding:** The 18-month gap between data recovery and victim notification (March 2026) highlights the extreme resource drain of post-breach forensic auditing for small municipalities.

**Strategic Precedent:** Proves that legal intervention can effectively reclaim stolen assets if the "data drop" occurs within US-governed cloud infrastructure.
