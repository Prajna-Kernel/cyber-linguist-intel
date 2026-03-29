**Incident #001 — March 28, 2026**

**Target:** Kash Patel (FBI Director) — Personal Email | Stryker Corporation (Fortune 500, Medical Devices)

**Sector:** Law Enforcement / Healthcare & Defence Manufacturing

**Threat Actor:** Handala Hack Team

**Origin:** Iran — Ministry of Intelligence and Security (MOIS)
Also tracked as: Banished Kitten | Cobalt Mystique | Red Sandstorm | Void Manticore

**Source:** The Hacker News — March 28, 2026 | Reuters | Flashpoint | Check Point Research | Unit 42 (Palo Alto Networks)

**Attack Type:** Hack-and-Leak (FBI Director) + Destructive Wiper Attack (Stryker)

**Labels:** Hacktivism | State-Sponsored | Wiper Malware | Geopolitical | Critical Infrastructure

---

**Analysis**
Handala Hack Team — a MOIS-affiliated group operating under a hacktivist persona — executed two simultaneous operations on March 28, 2026.

**Operation 1 — FBI Director Email Breach:**
Handala successfully compromised the personal email account of FBI Director Kash Patel and leaked photos and documents publicly. The FBI confirmed the breach, noting the data was historical and contained no government information. The breach was likely enabled through phishing or compromised credentials obtained via infostealer malware.

**Operation 2 — Stryker Wiper Attack:**
Handala deployed wiper malware against Stryker Corporation, deleting large volumes of company data and wiping thousands of employee devices. This marks the first confirmed destructive wiper attack against a U.S. Fortune 500 company. Initial access was achieved through Microsoft Intune via compromised administrative credentials. Stryker has since contained the incident and dismantled the persistence mechanisms installed.

---

**Key Technical Indicators:**
- **Initial Access:** Phishing / Compromised Microsoft Intune admin credentials (infostealer-sourced)

- **Lateral Movement:** RDP

- **Payload:** Handala Wiper + Handala PowerShell Wiper (deployed via Group Policy logon scripts)

- **Evasion:** VeraCrypt (legitimate disk encryption used to complicate recovery)

- **C2:** Telegram (used to blend malicious traffic with legitimate platform activity)

- *Malware also capable of recording audio and screen during active Zoom sessions*

---

**Strategic Context**
These attacks were retaliatory — triggered by a U.S. court-authorised seizure of four MOIS-operated domains (handala-hack[.]to, justicehomeland[.]org, karmabelow80[.]org, handala-redwanted[.]to). The U.S. DOJ stated these domains were used for psychological operations, data leaks, and targeting of Iranian dissidents and Israeli government personnel.

The Stryker attack represents a documented shift: Handala is moving from espionage and data theft toward active destruction of Western infrastructure. Flashpoint has flagged this as a dangerous evolution in supply chain threats — disrupting a major medical device supplier creates cascading risk across the entire healthcare ecosystem.

*The U.S. government is offering a $10 million reward for information on Handala members.*
