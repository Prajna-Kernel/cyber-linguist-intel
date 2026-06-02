# 🛡️ Threat Intelligence Log — June 2026

> Active monitoring of global cyber incidents.
> Focus: Russian-language threat actors and European infrastructure.
> Language access: Russian (A2→B2) | German (planned)

---

# Incident #001 — June 3, 2026

**Target:** Ukrainian government, military, and critical infrastructure organizations

**Sector:** Government / Military / Critical Infrastructure

**Threat Actor:** Gamaredon (aka Primitive Bear, Shuckworm, UAC-0010, Armageddon) — FSB-linked, Centre 18

**Origin:** Russia — Federal Security Service (FSB) Centre 18 — state-sponsored espionage and sabotage

**Source:** The Hacker News — June 2, 2026 | Sekoia

**Attack Type:** WinRAR CVE Exploitation → GammaPhish HTA → GammaLoad → GammaWorm/GammaSteel Deployment

**Labels:** Gamaredon | Shuckworm | CVE-2025-8088 | GammaPhish | GammaLoad | GammaWorm | GammaSteel | GammaWipe | WinRAR | Telegram DDR | NTFS ADS | USB Worm | AWS S3 | Ukraine | FSB

---

## Analysis

Sekoia published research on June 2, 2026 documenting a January 2026 Gamaredon campaign exploiting CVE-2025-8088, a path traversal flaw in WinRAR, to deliver a new malware suite against Ukrainian targets. The infection chain starts with a booby-trapped RAR archive that triggers GammaPhish — an HTA payload — which drops GammaLoad, a VBScript downloader that fingerprints the host, updates registry-based network configuration using dead drop resolvers, and fetches arbitrary VBScript payloads from C2.

GammaLoad delivers two distinct second-stage payloads depending on operator objectives. GammaWorm is a VBScript worm that establishes persistence via scheduled tasks, hides legitimate directories on network shares and USB drives, and replaces them with malicious LNK files that execute arbitrary code from C2. It resolves its C2 by sending a GET request via curl to a hard-coded public Telegram channel — blending into legitimate traffic. GammaWorm also uses NTFS Alternate Data Streams to hide its core modules from standard directory listings. GammaSteel is a modular information stealer that captures files matching target extensions and exfiltrates to an AWS S3 bucket, with an attacker-controlled server as fallback. GammaWipe, a wiper variant, can also be delivered through the same chain depending on tasking.

Sekoia characterized the architecture as resilient, massively scalable, and highly obfuscated — built to allow operators to update configurations on the fly. The modular design means the chain will almost certainly be reused across future campaigns.

---

## Key Technical Indicators:
- **CVE:** CVE-2025-8088 — WinRAR path traversal flaw — exploited via booby-trapped RAR archive
- **Stage 1:** GammaPhish — HTA payload launched via WinRAR exploitation
- **Stage 2:** GammaLoad — VBScript downloader — host fingerprinting, DDR-based C2 config, arbitrary VBScript fetch
- **Stage 3a:** GammaWorm — VBScript worm — scheduled task persistence, LNK replacement on network shares and USB drives, C2 via Telegram DDR, NTFS ADS module concealment
- **Stage 3b:** GammaSteel — modular infostealer — file exfiltration to AWS S3 bucket (attacker server fallback)
- **Stage 3c:** GammaWipe (aka GamaWiper) — wiper — delivered via same chain depending on operator tasking
- **C2 resolution:** curl GET request to hard-coded public Telegram channel
- **Stealth:** NTFS Alternate Data Streams used to hide GammaWorm core modules
- **Propagation vector:** USB drives and network shares via LNK file replacement
- **Architecture:** modular, operator-updatable configuration — assessed for long-term reuse

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: CVE-2025-8088 WinRAR path traversal exploited via malicious RAR archive to launch GammaPhish HTA payload
- **TA0002 — Execution**
  - T1059.005 — Command and Scripting Interpreter: Visual Basic: GammaLoad and GammaWorm are VBScript-based — execution via HTA and scheduled task triggers
  - T1204.002 — User Execution: Malicious File: victim opens booby-trapped RAR archive initiating the infection chain
- **TA0003 — Persistence**
  - T1053.005 — Scheduled Task/Job: Scheduled Task: GammaWorm establishes persistence via scheduled tasks on compromised hosts
- **TA0005 — Defense Evasion**
  - T1564.004 — Hide Artifacts: NTFS File Attributes: GammaWorm conceals core modules using NTFS Alternate Data Streams
  - T1102 — Web Service: GammaWorm resolves C2 via curl GET request to a public Telegram channel — blends with legitimate traffic
- **TA0008 — Lateral Movement**
  - T1091 — Replication Through Removable Media: GammaWorm hides legitimate directories on USB drives and replaces them with malicious LNK files executing arbitrary C2-retrieved code
- **TA0009 — Collection**
  - T1005 — Data from Local System: GammaSteel captures files matching target extensions from compromised hosts
- **TA0010 — Exfiltration**
  - T1567.002 — Exfiltration Over Web Service: Exfiltration to Cloud Storage: GammaSteel exfiltrates collected files to an AWS S3 bucket with attacker-controlled server as fallback
- **TA0040 — Impact**
  - T1485 — Data Destruction: GammaWipe wiper variant deliverable via same infection chain depending on operator tasking

---

## Strategic Context

Gamaredon is the highest-volume Russian APT operating against Ukraine. They are not subtle — the group prioritizes scale and persistence over stealth. The Telegram-based C2 resolution is a deliberate choice to blend into traffic that Ukrainian networks can't block without disrupting legitimate communications.

The USB propagation through LNK replacement is particularly relevant in a warzone context where devices move between air-gapped and internet-connected environments. A single compromised machine near frontline operations could propagate laterally through USB drives to systems that never touch the internet. The ability to swap between GammaSteel for collection and GammaWipe for destruction from the same chain is the clearest signal of dual espionage-sabotage tasking.

---

## Russian Language Context

Gamaredon действует под руководством ФСБ (FSB) — Федеральной службы безопасности (Federal'noy sluzhby bezopasnosti) — and specifically targets украинские государственные структуры (ukrainskiye gosudarstvennyye struktury) — Ukrainian state structures. The use of съёмные носители (s"yomnyye nositeli) — removable media — for propagation reflects persistent GRU and FSB interest in penetrating изолированные сети (izolirovannyye seti) — air-gapped networks — in Ukrainian military environments.
