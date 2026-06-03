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

---

# Incident #002 — June 4, 2026

**Target:** Ukrainian military, government, civilian, and business organizations; secondary targeting of Ukraine-related entities globally

**Sector:** Government / Military / Civil Society / Business

**Threat Actor:** GREYVIBE — previously undocumented Russian-speaking threat group, Kremlin-aligned

**Origin:** Russia — assessed Russian-speaking operators in Moscow time zone (UTC+3) — moderate confidence state alignment, low-to-moderate confidence cybercriminal membership involvement

**Source:** The Hacker News — May 29, 2026 | WithSecure (Mohammad Kazem Hassan Nejad)

**Attack Type:** Multi-Vector — Spear-Phishing / Fake CAPTCHA / Spoofed Ukrainian Sites → Custom Malware Suite — AI-Assisted Development and Operations

**Labels:** GREYVIBE | FallSpy | LegionRelay | PrincessClub | DroneLink | DAYLIGHT | TEASOUP | PhantomMail | ZAPiXDESK | WireGuard | AI-Assisted | Ukraine | ClickFix | KongTuke | TrickBot | UAC-0098 | XMRig | Kremlin-aligned

---

## Analysis

WithSecure published research on May 28, 2026 documenting GREYVIBE, a previously undocumented Russian-speaking threat group that has been persistently targeting Ukraine since at least August 2025. The group is assessed with moderate confidence to be Kremlin-aligned based on targeting, operator time zone, Russian-language code comments and admin panels, and intelligence collection focus consistent with the ongoing war. Ties to TrickBot gang infrastructure and UAC-0098 complicate clean state attribution — WithSecure assesses with low-to-moderate confidence that current or former cybercriminal members are involved.

The group runs multiple parallel infection chains. PhantomMail uses spear-phishing emails. Separate chains use fake CAPTCHA pages and fraudulent Ukrainian adult club websites as lure infrastructure. A distinct cluster named DroneLink used websites impersonating Ukrainian charitable foundations supporting FPV drone and UAV initiatives — targeting personnel likely involved in drone operations. March–April 2026 activity linked DroneLink to PrincessClub via shared C2 infrastructure, shared post-compromise tooling including WireGuard and ZAPiXDESK, and DAYLIGHT-obfuscated LegionRelay scripts hosted on the charity sites. GREYVIBE also ran a cluster impersonating "СПО НЕБО" (SPO NEBO), a Russian-language reference to airspace monitoring software.

What distinguishes GREYVIBE is systematic AI use across the entire operation. WithSecure identified clear markers of ChatGPT, Google Gemini, and Ideogram AI usage across lure creation, malware development, infrastructure setup, and command generation. Custom obfuscators DAYLIGHT and TEASOUP — both assessed as LLM-assisted — replaced earlier tools LOOKVALPS and LOOKVALJS from October 2025 and March 2026 respectively. The group uploads development and test samples to VirusTotal and uses internet slang in artifact naming conventions like "letsrollboyos" and "cuteuwu" — consistent with a younger, less disciplined operator profile. A small number of LegionRelay-infected machines were also used to deploy the XMRig cryptominer, a financially motivated side operation.

---

## Key Technical Indicators:
- **Active since:** August 2025 — ongoing as of May 2026
- **Infection vectors:** PhantomMail spear-phishing, fake CAPTCHA pages, fraudulent Ukrainian adult club sites, fake Ukrainian drone charity websites
- **Custom malware:** FallSpy, LegionRelay, PrincessClub, DroneLink cluster
- **Custom obfuscators:** DAYLIGHT (active Oct 2025), TEASOUP (active Mar 2026) — replaced LOOKVALPS and LOOKVALJS — LLM-assisted development assessed
- **Post-compromise tools:** WireGuard, ZAPiXDESK, DWAgent
- **AI tools confirmed:** ChatGPT, Google Gemini, Ideogram AI — used across lure creation, development, and C2 setup
- **Cybercrime overlaps:** TrickBot gang ISO builders, UAC-0098 — PhantomRelay variants in Microsoft Teams voice phishing (Jul 2025–Feb 2026) and KongTuke ClickFix chain (Feb–Mar 2026)
- **Side operation:** XMRig miner deployed to small number of LegionRelay-infected machines
- **Operator indicators:** Russian-language code comments, Russian admin panels, UTC+3 Moscow time zone, Russian↔Ukrainian translation artifacts
- **OPSEC failures:** VirusTotal test uploads, internet slang artifact naming, exposed development infrastructure
- **Attribution:** WithSecure — moderate confidence Kremlin-aligned, low-to-moderate confidence cybercriminal membership

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.001 — Phishing: Spear-phishing Attachment: PhantomMail chain delivers malware via spear-phishing emails
  - T1608.005 — Stage Capabilities: Link Target: fake CAPTCHA pages and fraudulent Ukrainian websites used as lure infrastructure to deliver malware to victims
- **TA0002 — Execution**
  - T1204.001 — User Execution: Malicious Link: victims interact with fake CAPTCHA pages, spoofed charity sites, and adult sites triggering payload delivery
- **TA0003 — Persistence**
  - T1219 — Remote Access Software: WireGuard VPN and ZAPiXDESK deployed post-compromise for persistent remote access and operator connectivity
- **TA0005 — Defense Evasion**
  - T1027.013 — Obfuscated Files or Information: Encrypted/Encoded File: DAYLIGHT and TEASOUP custom obfuscators applied to both initial-stage and post-compromise payloads — LLM-assisted development assessed
  - T1036.005 — Masquerading: Match Legitimate Name or Location: DroneLink cluster impersonates Ukrainian charitable foundations supporting FPV drone initiatives
- **TA0011 — Command and Control**
  - T1102 — Web Service: LegionRelay scripts hosted on fake charity domains — DAYLIGHT-obfuscated — used for C2 communication via legitimate-looking web infrastructure
- **TA0040 — Impact**
  - T1496 — Resource Hijacking: XMRig cryptominer deployed to a subset of LegionRelay-infected machines as a financially motivated side operation
- **TA0042 — Resource Development**
  - T1587.001 — Develop Capabilities: Malware: ChatGPT, Gemini, and Ideogram AI used across malware development, obfuscator creation, lure generation, and C2 setup — systematic AI integration across the full kill chain

---

## Strategic Context

GREYVIBE is the clearest example yet of what AI assistance looks like for a mid-tier threat group. This is not a sophisticated APT — WithSecure explicitly noted OPSEC failures, test samples on VirusTotal, and immature operational habits. But the AI integration is real and systematic. DAYLIGHT and TEASOUP are custom obfuscators assessed as LLM-assisted, lures are AI-generated, and the group is using AI to resolve infrastructure and C2 setup problems it couldn't otherwise solve independently.

The DroneLink cluster is the most tactically interesting element. Fake Ukrainian drone charity websites specifically targeting personnel involved in FPV drone operations reflects precise intelligence about what kinds of social engineering work against Ukraine's frontline drone ecosystem. That targeting precision — combined with WireGuard and ZAPiXDESK for quiet persistent access — suggests the objective is long-term intelligence collection on Ukrainian drone capabilities and operators.

The cybercriminal overlap with TrickBot and UAC-0098 fits the broader pattern of Russian intelligence services absorbing or tasking criminal infrastructure for state objectives. Clean attribution may never be possible here, which might be intentional.

---

## Russian Language Context

GREYVIBE operates in московском часовом поясе (moskovskom chasovom poyase) — Moscow time zone — with Russian-language комментарии в коде (kommentarii v kode) — code comments — and административные панели (administrativnyye paneli) — admin panels. The DroneLink cluster specifically targets украинских операторов дронов (ukrainskikh operatorov dronov) — Ukrainian drone operators — through поддельные благотворительные сайты (poddel'nyye blagotvoritel'nyye sayty) — fake charity websites.
