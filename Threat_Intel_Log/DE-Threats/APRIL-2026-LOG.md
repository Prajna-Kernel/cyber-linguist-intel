# 🛡️ Threat Intelligence Log — April 2026

> Active monitoring of European cyber infrastructure and vulnerability disclosures.
> Focus: EU-sector exposures, BSI advisories, and critical software flaws.
> Language access: German (planned B2) | English

---

# ⚠️ Cross-Sector Alert: Qilin Ransomware — Die Linke (German Political Party)
**Status:** Confirmed Breach — Dark Web Claim Active (April 1, 2026)
**Target:** Die Linke federal party infrastructure, Germany
**Note:** Russian-speaking Qilin RaaS group hit Germany's third major political party in three years. Internal org data and HQ employee PII at risk of publication. Member file unaffected. Party explicitly described attack as hybrid warfare.

👉 **[Read Full Technical Briefing in RU-Threats](../RU-Threats/APRIL-2026-LOG.md#incident-001)**

---

# ⚠️ Cross-Sector Alert: TA416 (China-nexus) — European Government & Diplomatic Targeting
**Status:** Active Campaign — ongoing since mid-2025, confirmed European diplomatic missions
**Target:** EU and NATO-affiliated diplomatic missions across Europe, including Germany-based entities
**Note:** PlugX backdoor delivered via OAuth redirect abuse and MSBuild-based C# loaders. Campaign expanded to Middle East post-US-Israel-Iran conflict escalation. Full technical breakdown in Global-Watch.

👉 **[Read Full Technical Briefing in Global-Watch](../Global-Watch/APRIL-2026-LOG.md#incident-004)**

---

# Incident #001 — April 8, 2026

**Target:** German Organizations (130+ ransomware cases)

**Sector:** Cross-sector — Enterprise / Government / Industrial

**Threat Actor:** REvil / GandCrab (leadership identified)

**Origin:** Russia

**Source:** BleepingComputer — April 6, 2026

**Attack Type:** Ransomware (RaaS — Attribution Update)

**Labels:** REvil | GandCrab | Russia | RaaS | BKA | Cybercrime

---

## Analysis
German Federal Police (BKA) identified two Russian nationals behind GandCrab and REvil operations.
The actors are linked to ransomware campaigns between 2019–2021, including over 130 cases in Germany. Both groups operated under a RaaS model with affiliate-based deployment.
This is an attribution development, not a new campaign.

---

## Key Technical Indicators:

- **Actors:** Daniil Shchukin (UNKN), Anatoly Kravchuk
- **Operations:** GandCrab → REvil
- **Model:** RaaS (affiliate structure)
- **Impact** (Germany): 130+ cases
- **Status:** Suspects located in Russia

---

## Strategic Context
GandCrab and REvil established the modern ransomware model still in use today.
Affiliate-based operations and scaled deployment remain standard across current ransomware groups.

---

## Russian Language Context
REvil and GandCrab operated through Russian-language forums, panels, and affiliate communications. Most operational coordination and tooling references were in Russian — useful exposure to real-world cybercrime Russian rather than formal language.

---

# Incident #002 — April 8, 2026

**Target:** ChipSoft — Dutch Healthcare IT provider (HiX Electronic Patient Record system)

**Sector:** Healthcare IT / Critical Infrastructure

**Threat Actor:** Unknown — unattributed ransomware group

**Origin:** Unknown

**Source:** BleepingComputer — April 8, 2026 | The Register | Z-CERT Advisory | NL Times

**Attack Type:**  Ransomware — Supply Chain Impact on National Healthcare Infrastructure

**Labels:** Ransomware | Healthcare | Supply Chain | EHR | Netherlands | Z-CERT | VPN Disconnect | Patient Data | Critical Infrastructure

---

## Analysis

On April 7, 2026, ChipSoft — the dominant provider of Electronic Health Record (EHR) software to Dutch hospitals — was hit by a ransomware attack that took its website offline and triggered an emergency advisory from Z-CERT, the Netherlands' dedicated healthcare cybersecurity authority. ChipSoft supplies its HiX system to approximately 70–80% of all Dutch hospitals and general practitioners, making it a single point of failure for the country's healthcare IT infrastructure.

Z-CERT advised all connected healthcare institutions to immediately disconnect their VPN connections to ChipSoft to prevent lateral movement of the ransomware into hospital networks. At least 11 hospitals proactively took their patient portals offline as a precaution. ChipSoft confirmed a "data incident" involving "possible unauthorized access" and could not rule out that patient data had been accessed or stolen.

The threat actor is unknown and no ransomware group has claimed responsibility as of reporting. The initial access vector has not been disclosed. The attack affected ChipSoft's cloud tenant for GP software, HiX on-premise, HiX SaaS, and the SaaS Patient Portal simultaneously — indicating the breach reached across multiple product environments.

---

## Key Technical Indicators:
- **Attack confirmed:** April 7, 2026 — ChipSoft website taken offline
- **Systems affected:** HiX on-premise, HiX SaaS, SaaS Patient Portal, GP software cloud tenant
- **Z-CERT advisory:** immediate VPN disconnection from ChipSoft recommended to all connected institutions
- **Hospitals disconnected:** at least 11 proactively severed connections
- **Data status:** unauthorized access confirmed — patient data exfiltration not ruled out
- **Threat actor:** unknown — no group has claimed responsibility
- **Initial access vector:** undisclosed
- **Investigation:** ongoing — National Cyber Security Centre and Z-CERT coordinating with ChipSoft

---

**MITRE ATT&CK Tactics:**
- **TA0001 — Initial Access** — ChipSoft network breached; initial access vector undisclosed
- **TA0002 — Execution** — ransomware payload executed across multiple product environments simultaneously
- **TA0008 — Lateral Movement** — ransomware spread assessed across HiX on-premise, HiX SaaS, SaaS Patient Portal, and GP cloud tenant; Z-CERT issued emergency VPN disconnection advisory to prevent further spread into connected hospital networks
- **TA0010 — Exfiltration** — patient data exfiltration not ruled out; unauthorized access confirmed — unconfirmed as of reporting
- **TA0040 — Impact** — ransomware encryption across multiple environments; ChipSoft website offline; 11+ hospitals severed connections; national healthcare EHR infrastructure disrupted

---

## Strategic Context

ChipSoft holds 70–80% market share in Dutch hospital EHR software. A single ransomware attack on this one vendor disrupted the patient record infrastructure of the majority of the Netherlands' hospitals simultaneously. This is not a one-hospital breach — it is a national healthcare supply chain incident.

The dependency is structural. ChipSoft has historically enforced gag clauses preventing hospitals from sharing security information with each other, sued hospitals that attempted to switch vendors, and holds a monopoly that the Dutch Authority for Consumers and Markets (ACM) investigated in 2023. Hospitals cannot easily leave even after an incident like this. Single-vendor dependency at national scale is a critical infrastructure vulnerability that no security tool can fix — it requires policy intervention.

Z-CERT's own annual landscape report identified ransomware as the foremost threat to Dutch healthcare. Belgium's AZ Monica hospital network in Antwerp was paralyzed in January 2026, forcing staff to turn away ambulances and transfer critical patients. ChipSoft follows that pattern at a larger scale — one vendor, one attack, national impact.

This connects to the broader healthcare ransomware pattern documented in this log: AMHC (RU-Threats MARCH-2026-LOG #001, Qilin) and Children's Council SF (Global-Watch APRIL-2026-LOG #007, SafePay). Ransomware groups consistently target healthcare because operational disruption creates maximum pressure to pay, and sensitive patient data provides secondary extortion leverage.
