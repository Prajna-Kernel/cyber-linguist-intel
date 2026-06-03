# 🛡️ Threat Intelligence Log — June 2026

> Active monitoring of cross-border cyber operations and state-sponsored activity.
> Focus: Non-regional incidents, wiper attacks, and AI-assisted threat tooling.
> Language access: English

---

# Incident #001 — June 3, 2026

**Target:** Government, research, academic, technology, and financial services organizations in Czech Republic and Taiwan; secondary targeting in Panama (Dec 2025), Cambodia, and South Korea (Jan 2026)

**Sector:** Government / Research / Technology / Financial Services / Critical Infrastructure

**Threat Actor:** Unnamed China-aligned APT — unattributed; broader campaign includes ESET-linked SteppeDriver, PhiliKit, NegativeGlimmer clusters

**Origin:** China-aligned — espionage motivated, ESET and Seqrite Labs attribution

**Source:** The Hacker News — June 1, 2026 | Seqrite Labs (Priya Patel) | ESET (Jean-Ian Boutin)

**Attack Type:** Spear-Phishing → LNK/EXE Dual-Path → RUSTCLOAK Rust Loader → AZUREVEIL AdaptixC2 Agent

**Labels:** Operation Dragon Weave | AdaptixC2 | AZUREVEIL | RUSTCLOAK | Azure Blob Storage | Cobalt Strike | China-aligned | Czech Republic | Taiwan | DLL Side-Loading | Rust Loader | Spear-Phishing | European Infrastructure

---

## Analysis

Seqrite Labs published research documenting Operation Dragon Weave, a China-aligned espionage campaign first observed targeting Taiwan in March 2026 and Czech Republic shortly after. Targets span government, research, academic, technology, and financial sectors. The campaign uses spear-phishing emails delivering ZIP archives containing files with Traditional Chinese filenames masquerading as official documents — one example translating to "Project Application Review Result Notification."

The infection chain has two delivery paths. Path A triggers when the victim opens a malicious LNK file disguised as a PDF, initiating a PowerShell-based dropper chain. Path B activates when the victim runs an executable disguised as a legitimate file. Both paths converge on RUSTCLOAK, a Rust-based loader that decrypts and executes the final payload — AZUREVEIL, a custom AdaptixC2 agent. AZUREVEIL uses Microsoft Azure Blob Storage as a dead-drop C2 channel and supports 36 commands covering remote control, data exfiltration, and further payload delivery. A decoy document is displayed to the victim in both paths to maintain the illusion of a legitimate file open.

ESET linked the broader campaign to multiple China-aligned clusters operating simultaneously — SteppeDriver, PhiliKit, and NegativeGlimmer — with some iterations switching from AdaptixC2 to Cobalt Strike in January 2026. A December 2025 instance targeted a Panamanian government organization using DLL side-loading initiated via spear-phishing. ESET's Jean-Ian Boutin noted that South Korean targeting aligns with Beijing's sustained interest in technologies prioritized under the Made in China 2025 industrial policy.

---

## Key Technical Indicators:
- **Campaign first observed:** March 26, 2026 — first VirusTotal submission from Taiwan
- **Delivery:** spear-phishing ZIP archive — Traditional Chinese filenames — PDF-masquerading LNK and executable
- **Path A:** LNK → PowerShell dropper chain → RUSTCLOAK → AZUREVEIL
- **Path B:** Executable → RUSTCLOAK → AZUREVEIL (decoy document displayed)
- **RUSTCLOAK:** Rust-based loader — decrypts and loads AZUREVEIL from encrypted containers (1.dat, Com.dat)
- **AZUREVEIL:** custom AdaptixC2 agent — Azure Blob Storage dead-drop C2 — 36 supported commands
- **January 2026 iterations:** AdaptixC2 replaced with Cobalt Strike — additional victims in Cambodia and South Korea
- **December 2025:** Panama government organization targeted via DLL side-loading spear-phishing chain
- **Related clusters:** SteppeDriver, PhiliKit, NegativeGlimmer (ESET attribution)
- **Czech Republic targeting:** European government infrastructure — NATO member state

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.001 — Phishing: Spear-phishing Attachment: ZIP archives with Traditional Chinese PDF-masquerading filenames delivered via targeted phishing emails
- **TA0002 — Execution**
  - T1059.001 — Command and Scripting Interpreter: PowerShell: Path A uses PowerShell-based dropper chain to progress infection from LNK to RUSTCLOAK
  - T1204.002 — User Execution: Malicious File: victim opens LNK or executes disguised binary triggering the infection chain
- **TA0005 — Defense Evasion**
  - T1574.002 — Hijack Execution Flow: DLL Side-Loading: December 2025 Panama instance used DLL side-loading chain initiated via spear-phishing
  - T1036.007 — Masquerading: Double File Extension: LNK file disguised as PDF document using Traditional Chinese filename
  - T1027.013 — Obfuscated Files or Information: Encrypted/Encoded File: RUSTCLOAK decrypts payload from encrypted containers (1.dat, Com.dat) at runtime
- **TA0011 — Command and Control**
  - T1102.001 — Web Service: Dead Drop Resolver: AZUREVEIL uses Microsoft Azure Blob Storage as dead-drop C2 channel — 36 commands supported
- **TA0009 — Collection**
  - T1005 — Data from Local System: AZUREVEIL supports data exfiltration and remote control across compromised hosts via 36-command C2 framework

---

## Strategic Context

Czech Republic is a NATO member with significant defense and research infrastructure — targeting it alongside Taiwan signals this campaign is about strategic intelligence collection across both Western alliance and Indo-Pacific priorities simultaneously. The switch from AdaptixC2 to Cobalt Strike in January 2026 suggests the actor has access to multiple toolsets and adjusts based on operational context.

The use of Azure Blob Storage as a dead-drop C2 is the same pattern seen across multiple Chinese APT clusters — legitimate cloud infrastructure that can't be blocked without collateral impact on enterprise operations. RUSTCLOAK being written in Rust is notable given Rust's growing adoption among threat actors specifically for its resistance to memory analysis and detection.

---

# Incident #002 — June 4, 2026

**Target:** South Korean military and corporate entities; defense, government, and healthcare sectors

**Sector:** Defense / Government / Healthcare / Corporate

**Threat Actor:** Kimsuky (aka Velvet Chollima, APT43, Sparkling Pisces, Emerald Sleet, Black Banshee) — RGB-linked DPRK state-sponsored APT

**Origin:** North Korea — Reconnaissance General Bureau (RGB) — state-sponsored espionage

**Source:** The Hacker News — May 29, 2026 | ENKI | ASEC

**Attack Type:** Social Engineering → HTTPSpy RAT / HelloDoor / VS Code Tunnel C2 — Multi-Campaign Espionage

**Labels:** Kimsuky | Velvet Chollima | HTTPSpy | HelloDoor | HttpMalice | HappyDoor | AppleSeed | PebbleDash | VS Code Tunnels | Cloudflare Tunnels | DWAgent | Webex Lure | B2B Spoof | South Korea | DPRK | GPKI | Defense

---

## Analysis

ENKI published research on May 29, 2026 documenting Kimsuky campaigns against South Korean military and corporate entities throughout March and April 2026. The group ran multiple parallel social engineering operations, each using different lure themes and delivery mechanisms while converging on the same post-compromise objectives: remote access, data exfiltration, and credential theft.

The March 2026 campaign delivered HTTPSpy — a remote access trojan Kimsuky has been disguising as legitimate South Korean security software installers since 2023 — through a fake webpage impersonating the security software installation page of a South Korean B2B messaging service, targeting messaging administrators specifically. A separate April 2026 campaign used a counterfeit Cisco Webex page that replicated a legitimate meeting schedule, tricking victims into downloading an encrypted JavaScript file that deployed HTTPSpy. Both chains rely on the victim trusting the spoofed page enough to actively download and run the installer. Kimsuky also introduced two new malware families: HelloDoor and HttpMalice, alongside enhanced versions of AppleSeed called HappyDoor — which focuses specifically on data exfiltration and GPKI certificate extraction. PebbleDash variants were also deployed.

The most significant tactical development is the use of Visual Studio Code tunneling and Cloudflare Quick Tunnels for C2. VS Code tunneling is a legitimate developer feature that creates an encrypted tunnel between a VS Code instance and Microsoft's infrastructure — traffic is indistinguishable from normal developer activity and cannot be blocked without disrupting legitimate VS Code usage across an organization. DWAgent, an open-source remote monitoring tool, was deployed alongside for post-exploitation activities.

---

## Key Technical Indicators:
- **Campaign period:** March–April 2026
- **Lure 1**: fake B2B messaging service security software installation page — targeting messaging administrators
- **Lure 2:** counterfeit Cisco Webex meeting page — encrypted JavaScript payload delivery
- **Primary RAT:** HTTPSpy — disguised as South Korean security software installers since 2023
- **New malware:** HelloDoor, HttpMalice
- **Updated malware:** HappyDoor (AppleSeed variant) — data exfiltration + GPKI certificate extraction focus
- **Additional:** PebbleDash variants
- **C2 method 1:** Visual Studio Code tunneling — legitimate developer infrastructure — traffic indistinguishable from normal VS Code usage
- **C2 method 2:** Cloudflare Quick Tunnels — encrypted tunnel bypassing traditional C2 detection
- **Post-exploitation:** DWAgent remote monitoring tool — open-source, legitimate
- **Target sectors:** defense, government, healthcare, corporate
- **Attribution:** ENKI, ASEC — high confidence Kimsuky

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.002 — Phishing: Spear-phishing Link: counterfeit Webex meeting page and fake B2B security installer page deliver malicious payloads to targeted victims
- **TA0002 — Execution**
  - T1204.002 — User Execution: Malicious File: victims download and execute spoofed security software installers or encrypted JavaScript payload from fake Webex page
- **TA0006 — Credential Access**
  - T1552 — Unsecured Credentials: HappyDoor specifically targets GPKI certificate extraction alongside general data exfiltration from compromised hosts
- **TA0005 — Defense Evasion**
  - T1036.005 — Masquerading: Match Legitimate Name or Location: HTTPSpy disguised as legitimate South Korean security software installers — consistent tactic since 2023
  - T1090.003 — Proxy: Multi-hop Proxy: Cloudflare Quick Tunnels used to route C2 traffic through legitimate Cloudflare infrastructure evading network-based detection
- **TA0011 — Command and Control**
  - T1102 — Web Service: Visual Studio Code tunneling used as C2 channel — encrypted tunnel through Microsoft infrastructure indistinguishable from legitimate developer activity
  - T1219 — Remote Access Software: DWAgent open-source remote monitoring tool deployed for persistent post-exploitation remote access
- **TA0009 — Collection**
  - T1005 — Data from Local System: HappyDoor and HTTPSpy collect files and credentials from compromised hosts for exfiltration

---

## Strategic Context

VS Code tunneling as a C2 channel is the most operationally significant detail here. It's not a vulnerability — it's a legitimate feature being repurposed. Organizations that block known C2 infrastructure, malicious domains, or suspicious outbound connections have no clean way to block VS Code tunnel traffic without breaking legitimate developer workflows. Kimsuky is deliberately choosing tools that defenders can't easily action against.

The consistent targeting of South Korean defense, government, and healthcare across multiple parallel campaigns in a single two-month period reflects sustained tasking from RGB rather than opportunistic activity. GPKI certificate extraction via HappyDoor is specifically targeted at South Korea's Government Public Key Infrastructure — credentials that open doors into government network services beyond just the compromised endpoint.

---
