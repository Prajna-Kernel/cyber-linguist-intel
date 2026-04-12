# 🛡️ Threat Intelligence Log — April 2026

> Active monitoring of global cyber incidents.
> Focus: Russian-language threat actors and European infrastructure.
> Language access: Russian (A2→B2) | German (planned)

---

# Incident #001 — April 4, 2026

**Target:** Die Linke (The Left Party) — German Federal Parliament / State Governments

**Sector:** Political Organization / Democratic Infrastructure

**Threat Actor:** Qilin (Russian-speaking RaaS group)

**Origin:** Russia-nexus (assessed — financially and politically motivated)

**Source:** BleepingComputer — April 4, 2026 | Heise Online | ransomware.live

**Attack Type:** Ransomware + Data Exfiltration (Double Extortion) — Hybrid Warfare Context

**Labels:** Ransomware | Qilin | RaaS | Political Targeting | Germany | Double Extortion | Hybrid Warfare | Dark Web Leak

---

## Analysis

On March 26, 2026, Qilin ransomware compromised the network infrastructure of Die Linke, Germany's democratic socialist party currently holding 64 seats in the Bundestag with 123,000 registered members. The party detected anomalies and proactively took systems offline the same day to contain spread. On April 1, Qilin publicly claimed the attack on their dark web leak site, listing Die Linke as a victim without immediately publishing data samples — standard pressure mechanics to coerce payment.

Die Linke explicitly framed this as political targeting, not opportunistic crime. Their federal managing director described it as consistent with hybrid warfare operations: ransomware used to intimidate, discredit, and weaken democratic structures rather than purely for financial gain. The party stated the attack "does not appear to be coincidental in this context" — a direct reference to the broader pattern of Russian interference in German political infrastructure.

This is the third major German political party to be hit in recent years. SPD was compromised in early 2023 (later attributed to Russia). CDU was hit via a perimeter zero-day in May 2024 (now under Federal Prosecutor General investigation for suspected espionage/sabotage). Die Linke completes a sweep across the German political spectrum — left, center-right, and now democratic socialist.

Member data is reported as unaffected. Internal party organization data and employee personal information at headquarters are at risk of publication.

---

## Key Technical Indicators:

- **Compromise date:** March 26, 2026 — anomalies detected, systems proactively taken offline
- **Qilin dark web claim:** April 1, 2026 — no data samples published as of reporting
- **Data at risk:** internal party organization data, personal information of headquarters employees
- **Member file:** confirmed unaffected
- *Criminal complaint filed with German police*
- *Coordinating with BSI and independent security experts*
- *No ransom amount or payment status disclosed*

---

## Strategic Context

Qilin is assessed as a Russian-speaking RaaS group with a dual motivation profile — financial extortion combined with politically aligned target selection. Hitting Die Linke, a party with significant eastern German presence and a historically complex relationship with Russian political narratives, carries symbolic weight beyond financial leverage.

The German political party targeting pattern is now systematic across three election cycles and three major parties. Each incident has involved different TTPs but consistent strategic logic: disruption, data exposure, and reputational damage to democratic institutions. This represents a mature hybrid warfare playbook being executed at scale against EU member state political infrastructure.

**For RU-Threats context:** Qilin has previously appeared in this log targeting AMHC (Maine mental health, March 27). The same group operating against a US healthcare target and a German political party in the same week illustrates the breadth of their operational tempo.

---

## Russian Language Context

Qilin operates within the Russian-speaking cybercriminal ecosystem. Dark web forum activity, ransom notes, and leak site communications are conducted in Russian. The group's dual motivation — financial and political — is consistent with tolerated or loosely directed RaaS operations within the RU-nexus, where criminal groups pursue targets aligned with state interests without formal coordination.


---

# Incident #002 — April 6, 2026

**Target:** CERT-UA and Ukrainian government entities

**Sector:** Government / Cybersecurity

**Threat Actor:** UAC-0255 (Russian-nexus)

**Origin:** Russia (high confidence)

**Source:** CERT-UA | Microsoft Threat Intelligence | The Hacker News - April 1, 2026

**Attack Type:** Impersonation + malware delivery

**Labels:** AGEWHEEZE malware | 1 million emails | Spear-phishing | Russian-nexus

---

## Analysis
Russian-linked group UAC-0255 ran a massive impersonation campaign, sending over 1 million emails pretending to be official CERT-UA cybersecurity alerts. The emails delivered AGEWHEEZE malware for persistent access and data theft.

---

## Key Technical Indicators:
**Delivery:** Mass spear-phishing emails impersonating CERT-UA
**Malware:** AGEWHEEZE
**Goal:** Persistent access and intelligence collection
**Scale:** Over 1 million emails sent

---

## Strategic Context
By impersonating CERT-UA itself, the attackers exploited trust in official channels during heightened tensions. This is a classic Russian-nexus operation aimed at long-term access inside Ukrainian government networks.

---

### Russian Language Context
The phishing emails and AGEWHEEZE malware contained Russian-language strings and command structures typical of Russian-speaking actors. Excellent real-world practice for reading operational Russian used in actual threat communications.

---

# ⚠️ Cross-Sector Alert: REvil / GandCrab — Germany Attribution  
**Status:** Attribution Confirmed — BKA Identification (April 6, 2026)  
**Target:** German organizations (130+ historical ransomware cases)  
**Note:** German authorities identified Russian nationals behind REvil and GandCrab operations.  

👉 **[Read Full Technical Briefing in DE-Threats](../DE-Threats/APRIL-2026-LOG.md#incident-001)**

---

# Incident #003 — April 9, 2026

**Target:** Adobe Reader users — targeted via malicious PDF documents (Russian oil & gas sector lures)

**Sector:** End-User Software / Cross-Sector (any Adobe Reader user)

**Threat Actor:** Unknown — unattributed, assessed as targeted operation based on Russian-language lures

**Origin:** Unattributed — Russian nexus assessed (lure content references Russian oil & gas current events)

**Source:** BleepingComputer — April 9, 2026 | EXPMON (Haifei Li) | The Register | The Hacker News

**Attack Type:** Zero-Day Exploitation — Fingerprinting-style PDF exploit, data exfiltration + potential RCE/SBX

**Labels:**  Zero-Day | Adobe Reader | PDF | Unpatched | JavaScript | API Abuse | Fingerprinting | RCE | SBX | Russian Lure | Oil & Gas | No Patch Available

---

## Analysis

A zero-day vulnerability in Adobe Reader has been actively exploited since at least December 2025 — over four months before public disclosure. The flaw was discovered by Haifei Li, founder of sandbox-based exploit detection platform EXPMON, who described it as a highly sophisticated fingerprinting-style exploit. The first malicious sample, named "Invoice540.pdf," appeared on VirusTotal on November 28, 2025. A second sample was uploaded March 23, 2026.

The exploit triggers automatically upon opening the PDF — no additional user interaction required. It runs heavily obfuscated JavaScript that abuses two privileged Acrobat APIs: util.readFileIntoStream (file access) and RSS.addFeed (web feed handling). These are used to harvest local system data — OS information, language settings, file paths — and exfiltrate it to an attacker-controlled server (169.40.2[.]68:45191). The first stage is reconnaissance: profiling the victim before deciding whether to deploy follow-on payloads including RCE and sandbox escape (SBX) exploits.

The exploit works on the latest version of Adobe Reader. No patch is available as of reporting — Adobe was notified on April 7, 2026.

Researcher Gi7w0rm identified that the lure documents contain Russian-language content referencing current events in Russia's oil and gas sector — strongly suggesting targeted operation against individuals in or connected to that industry. This is not confirmed attribution but points to deliberate target selection.

---

## Key Technical Indicators:
- **Vulnerability:** unpatched zero-day in Adobe Reader — confirmed working on latest version
- **Trigger:** automatic on PDF open — zero additional user interaction
- **APIs abused:** util.readFileIntoStream, RSS.addFeed (privileged Acrobat APIs)
- **Stage 1:** data exfiltration — OS info, language settings, file paths → C2 server 169.40.2[.]68:45191
- **Stage 2 (potential):** RCE and sandbox escape exploits delivered via follow-on JavaScript
- **Lure filenames:** Invoice540.pdf (social engineering via fake invoice)
- **Lure content:** Russian-language, referencing Russian oil & gas current events
- **First VirusTotal sample:** November 28, 2025
- **Second sample:** March 23, 2026
- **Adobe notified:** April 7, 2026
- *No patch available as of disclosure*
- **Network detection:** block HTTP/HTTPS traffic containing "Adobe Synchronizer" in User-Agent header

---

## MITRE ATT&CK Tactics:
- **TA0001 — Initial Access** — zero-day exploit delivered via malicious PDF (Invoice540.pdf); triggers automatically on open; no user interaction required beyond opening the file
- **TA0002 — Execution** — obfuscated JavaScript executes automatically within Adobe Reader upon PDF open; abuses util.readFileIntoStream and RSS.addFeed APIs
- **TA0007 — Discovery** — Stage 1 harvests local system data: OS information, language settings, file paths; victim profiled before follow-on payload decision
- **TA0011 — Command & Control** — exfiltrated data transmitted to attacker-controlled server 169.40.2[.]68:45191
- **TA0010 — Exfiltration** — OS info, language settings, and file paths exfiltrated to C2 server as part of fingerprinting stage
- **TA0002 — Execution (Stage 2, potential)** — RCE and sandbox escape payloads assessed as follow-on capability; not confirmed delivered — included due to documented design intent

---

## Strategic Context

This exploit sat undetected for at least four months while actively harvesting data from victims. The fingerprinting-style approach — recon first, payload later — is characteristic of targeted intelligence collection operations rather than mass opportunistic attacks. The attacker is profiling victims before committing to full compromise, which conserves the zero-day and reduces detection risk.

Russian-language lures tied to oil and gas sector events narrow the likely target set considerably — energy sector professionals, executives, or analysts consuming Russia-related industry documents. This is a high-value target profile consistent with state-directed or state-adjacent intelligence collection.

The absence of a patch at time of disclosure makes this immediately actionable for defenders: do not open PDFs from untrusted sources, and block the "Adobe Synchronizer" User-Agent string at the network layer until Adobe releases a fix.

---

## Russian Language Context

The lure documents are crafted in Russian and reference ongoing events in Russia's oil and gas industry — a deliberate targeting signal. The use of Russian-language social engineering to deliver a zero-day exploit is consistent with operations targeting energy sector intelligence in the Russian-speaking ecosystem, whether by state actors, contractors, or criminal groups with state-adjacent tasking.

---

# Incident #004 — April 13, 2026

**Target:** Ukraine government and NATO allies — central executive bodies, defense, hydrometeorology, emergency services (Ukraine); rail logistics (Poland); maritime and transportation (Romania, Slovenia, Turkey); ammunition logistics partners (Slovakia, Czech Republic); military and NATO partners

**Sector:** Government / Defense / Critical Infrastructure / NATO Logistics

**Threat Actor:** APT28 (aka Pawn Storm, Fancy Bear, Forest Blizzard, UAC-0001) — Russian GRU-linked advanced persistent threat

**Origin:** Russia (GRU — assessed high confidence, Trend Micro attribution)

**Source:** The Hacker News — April 8, 2026 | Trend Micro Research (Hacquebord + Kakara) | Security Affairs | SC Media | CERT-UA

**Attack Type:** Spear-Phishing → Zero-Day Exploitation → PRISMEX Malware Suite Deployment (Espionage + Sabotage)

**Labels:** APT28 | Pawn Storm | PRISMEX | Spear-Phishing | Steganography | COM Hijacking | Zero-Day | CVE-2026-21509 | CVE-2026-21513 | Ukraine | NATO | Cloud C2 | Wiper | Espionage | Defense Supply Chain

---

## Analysis

Since at least September 2025, APT28 has been running a sustained spear-phishing campaign against Ukraine and its NATO allies, deploying a previously undocumented modular malware suite codenamed PRISMEX. The campaign was publicly disclosed April 8, 2026 via a Trend Micro technical report by researchers Feike Hacquebord and Hiroyuki Kakara. Targets span Ukraine's central government, defense sector, and emergency services — extending into NATO ally logistics chains across Poland, Romania, Slovakia, Czech Republic, Slovenia, and Turkey. The operational focus is the defense supply chain supporting Ukraine: rail logistics, ammunition initiatives, maritime transport, and military aid infrastructure.

The initial access vector is spear-phishing — emails themed around military training or aid deliver malicious RTF and Excel files. These exploit CVE-2026-21509, a security feature bypass in Microsoft Office's OLE mechanism, which forces the victim's system to retrieve a malicious .LNK file. That file then chains with CVE-2026-21513, a protection mechanism failure in the MSHTML Framework, to bypass security warnings and execute payloads without user interaction. CVE-2026-21513 was confirmed as a zero-day — an LNK exploit sample appeared on VirusTotal on January 30, 2026, eleven days before Microsoft's patch on February 10, 2026. Infrastructure preparation for CVE-2026-21509 began two weeks before its public disclosure, indicating APT28 had advance knowledge of the vulnerability.

## PRISMEX is a collection of interconnected components named for its steganographic technique of distributing payloads across image files:

- **PrismexSheet** — malicious Excel dropper with VBA macros; extracts payloads embedded via steganography; establishes persistence via COM DLL hijacking; displays realistic decoy content (Ukrainian drone inventories, supplier price lists, military logistics) after macros are enabled
- **PrismexDrop** — native dropper; prepares environment for follow-on exploitation; persistence via scheduled tasks and COM DLL hijacking
- **PrismexLoader** — bridges dropper and implant stages
- **PrismexStager** — COVENANT Grunt implant; abuses Filen.io cloud storage for encrypted C2 communications; assessed as an expansion of MiniDoor and NotDoor (aka GONEPOSTAL), APT28's Outlook backdoor deployed in late 2025

In at least one October 2025 incident, the COVENANT Grunt payload executed a destructive wiper command erasing all files under %USERPROFILE% — confirming dual espionage and sabotage capability. Alternate infection paths deploy MiniDoor, an Outlook email stealer, instead of the full PRISMEX suite.

---

## Key Technical Indicators:
- **Campaign active:** September 2025 — ongoing; publicly disclosed April 8, 2026
- **Initial vector:** spear-phishing — military/aid-themed RTF and Excel lures
- **CVE-2026-21509:** Microsoft Office OLE security feature bypass — exploited to retrieve malicious .LNK file
- **CVE-2026-21513:** MSHTML Framework protection mechanism failure — confirmed zero-day; 11-day window before patch (Jan 30 – Feb 10, 2026)
- **Infrastructure prep:** two weeks before CVE-2026-21509 disclosure — advance vulnerability knowledge confirmed
- **Malware suite:** PRISMEX — PrismexSheet, PrismexDrop, PrismexLoader, PrismexStager
- **Steganography:** payloads distributed across image files
- **Persistence:** COM DLL hijacking (HKCU InProcServer32), scheduled tasks
- **C2:** Filen.io cloud storage (PrismexStager / COVENANT Grunt) — legitimate service abused to blend with normal traffic
- **Alternate payload:** MiniDoor — Outlook email stealer
- **Wiper capability:** confirmed in October 2025 incident — erases %USERPROFILE% directory
- Shared infrastructure
- **indicator:** domain wellnesscaremed[.]com — links CVE-2026-21509 and CVE-2026-21513 exploitation
- **Attribution:** Trend Micro high confidence — tool lineage, infrastructure reuse, COM hijacking pattern, MiniDoor/NotDoor lineage, COVENANT framework continuity

---

## MITRE ATT&CK Tactics:
- **TA0001 — Initial Access** — spear-phishing emails with military/aid-themed RTF and Excel lures; CVE-2026-21509 exploited to retrieve malicious .LNK file; CVE-2026-21513 zero-day chained to bypass security warnings and execute without user interaction
- **TA0002 — Execution** — VBA macros in PrismexSheet execute steganographically embedded payloads; COVENANT Grunt implant executes via PrismexStager; wiper command executed in at least one confirmed incident
- **TA0003 — Persistence** — COM DLL hijacking via HKCU InProcServer32 modifications; scheduled tasks installed by PrismexDrop; MiniDoor/NotDoor Outlook backdoor deployed as alternate persistence
- **TA0005 — Defense Evasion** — steganography conceals payloads within image files; legitimate cloud service (Filen.io) abused for C2 to blend with normal traffic; decoy documents displayed post-execution to avoid victim suspicion; fileless execution techniques
- **TA0007 — Discovery** — victim environment profiled before follow-on payload decision; target selection based on role in Ukrainian defense supply chain
- **TA0009 — Collection** — MiniDoor targets Outlook email data; PrismexStager facilitates information gathering from compromised systems
- **TA0011 — Command & Control** — COVENANT Grunt communicates via Filen.io cloud storage using encrypted channels; legitimate service abuse makes C2 traffic difficult to distinguish from normal activity
- **TA0040 — Impact** — wiper command confirmed in October 2025 incident (erases %USERPROFILE%); dual espionage and sabotage capability documented; defense supply chain disruption as strategic objective

---

## Strategic Context
This campaign represents APT28 operating at full capability — zero-day exploitation, modular malware, steganographic delivery, cloud-based C2, and confirmed destructive capability running simultaneously against a strategically critical target set. The focus on Ukraine's defense supply chain — ammunition logistics, rail transport, military aid partners — is not espionage for its own sake. It is mapping and potentially pre-positioning to disrupt the operational backbone supporting Ukraine's war effort.

The advance knowledge of CVE-2026-21509 two weeks before disclosure and the confirmed zero-day exploitation of CVE-2026-21513 indicate APT28 operates with either insider access to vulnerability research pipelines or independent discovery capability at a level that compresses the defender's patch window to near zero.

This is the most technically sophisticated Russian operation documented in this log. Previous RU-Threats entries — Qilin ransomware against Die Linke (RU-Threats APRIL #001), Handala/MOIS operations (Global-Watch MARCH #001) — demonstrated Russian-nexus reach and political targeting. PRISMEX demonstrates something different: a mature, modular, persistent capability designed for long-term access and reversible-to-destructive operations against NATO military infrastructure.

The use of Filen.io for C2 is a detection evasion technique with direct implications for network defenders — blocking cloud storage services at the perimeter is not feasible, making behavioral anomaly detection the only reliable mitigation.

---

## Russian Language Context

APT28 operates under GRU Unit 26165 and is one of Russia's most documented state cyber actors. The PRISMEX campaign's targeting of Ukraine's defense supply chain and NATO logistics partners is consistent with GRU tasking during active conflict — disrupting Western military aid infrastructure serves direct battlefield objectives. The COVENANT framework's prior documentation by CERT-UA (June 2025) and the continuity of tool lineage from NotDoor through MiniDoor to PrismexStager confirms this is a sustained, resourced operation with institutional continuity, not an opportunistic campaign.
