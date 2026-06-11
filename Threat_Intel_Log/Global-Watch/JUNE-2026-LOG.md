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

# Incident #003 — June 9, 2026

**Target:** Unnamed organization — compromised via its Managed Services Provider (MSP); Egnyte Storage Sync appliance, Synology NAS, pfSense firewall

**Sector:** Managed Services / Enterprise / Network Infrastructure

**Threat Actor:** VerdantBamboo (aka Clay Typhoon, UNC5221, Warp Panda) — China-nexus cyber espionage

**Origin:** China — state-sponsored espionage, PRC-linked

**Source:** The Hacker News — June 8, 2026 | Volexity (Damien Cash, Paul Rascagneres, Steven Adair, Tom Lancaster)

**Attack Type:** MSP Supply Chain Compromise → Linux Appliance Backdoor → M365 Access via Proxy**

**Labels:** VerdantBamboo | Clay Typhoon | UNC5221 | BRICKSTORM | PLENET | GRIMBOLT | AGENTPSD | BSD | pfSense | Egnyte | Synology NAS | MSP Compromise | M365 | Conditional Access Bypass | Linux | EDR Evasion | China-nexus

---

## Analysis

Volexity published research on June 4, 2026 documenting a VerdantBamboo intrusion discovered during an incident response engagement in September 2025. The threat actor had compromised the victim through its MSP — specifically infecting the MSP's pfSense firewall with a BSD variant of BRICKSTORM, a backdoor previously only observed on Linux and Windows systems. The initial compromise of the victim's own Egnyte Storage Sync appliance occurred at least 18 months before discovery, with access maintained via IP addresses assigned through the victim's web SSL VPN.

VerdantBamboo used BRICKSTORM's proxying capabilities on the compromised Storage Sync system alongside stolen credentials to access the victim's Microsoft 365 environment — specifically to blend into legitimate network traffic and bypass Conditional Access policies. After initial remediation, the actor staged a return: using stolen administrative credentials to connect to the firewall, configure web SSL VPN access, and deploy additional malware to a Synology NAS appliance over SSH. The two payloads deployed to the NAS were PLENET (aka GRIMBOLT), a cross-platform .NET Core backdoor compiled with native AOT supporting interactive shell, remote command execution, file manipulation, and C2 switching; and AGENTPSD, a Python-based reverse shell acting as a fallback if the primary implant fails.

Volexity characterized VerdantBamboo as highly sophisticated, with strong knowledge of proprietary appliances allowing customized persistence mechanisms per device. The actor uses a limited number of domains and IPs per victim, and names implants uniquely per deployment — a level of operational discipline that significantly complicates cross-victim correlation. PLENET was separately reported by Google in February 2026 in connection with UNC6201 exploiting a CVSS 10.0 Dell RecoverPoint zero-day — confirming shared tooling across China-nexus clusters.

---

## Key Technical Indicators:
- **Initial access:** MSP compromise — BSD BRICKSTORM deployed to MSP's pfSense firewall
- **Victim entry point:** Egnyte Storage Sync local privilege escalation flaw — patched in version 13.13 (March 2026)
- **Access method:** SSL VPN IP addresses from victim organization's own address space
- **M365 access:** BRICKSTORM proxy + stolen credentials — bypasses Conditional Access policies
- **Post-remediation return:** stolen admin credentials used to reconfigure firewall SSL VPN and deploy to NAS via SSH
- **PLENET (aka GRIMBOLT):** .NET Core cross-platform backdoor — AOT compiled — interactive shell, RCE, file ops, C2 switching
- **AGENTPSD:** Python reverse shell — fallback implant
- **BRICKSTORM variant:** BSD-compiled — first observed on BSD/pfSense — previously Linux/Windows only
- **Operational discipline:** per-victim unique implant naming and persistence mechanisms — limited C2 infrastructure reuse
- **Actor overlaps:** UNC6201 (PLENET/Dell CVE-2026-22769), UNC5221, Clay Typhoon, Warp Panda

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1195.003 — Supply Chain Compromise: Compromise Hardware Supply Chain: victim compromised through MSP — BSD BRICKSTORM deployed to MSP pfSense firewall before lateral movement to victim environment
  - T1190 — Exploit Public-Facing Application: local privilege escalation flaw in Egnyte Storage Sync appliance exploited to deploy BRICKSTORM
- **TA0003 — Persistence**
  - T1505 — Server Software Component: customized per-device persistence mechanisms deployed on Storage Sync appliance, pfSense firewall, and Synology NAS — EDR-blind appliances specifically chosen
- **TA0005 — Defense Evasion**
  - T1078 — Valid Accounts: stolen administrative credentials used post-remediation to reconnect to firewall and reconfigure VPN access
  - T1090.002 — Proxy: External Proxy: BRICKSTORM proxying capabilities used to route M365 access through compromised appliance — traffic appears as legitimate organizational activity
- **TA0008 — Lateral Movement**
  - T1021.004 — Remote Services: SSH: PLENET and AGENTPSD deployed to Synology NAS over SSH following firewall access
- **TA0011 — Command and Control**
  - T1071.001 — Application Layer Protocol: Web Protocols: PLENET supports C2 server switching — multiple protocol options for resilient operator connectivity
  - T1095 — Non-Application Layer Protocol: AGENTPSD Python reverse shell operates as fallback C2 channel if primary implant stops functioning

---

## Strategic Context

The MSP entry point is the most significant operational detail here. VerdantBamboo didn't target the victim directly — it compromised the MSP first and used that trusted relationship as the bridge. MSPs have privileged access to client environments by design, making them high-value targets that effectively multiply the blast radius of a single compromise.

The 18-month undetected dwell time on the Storage Sync appliance reflects the deliberate choice to target devices that cannot run EDR software. Volexity explicitly noted this as a design principle of VerdantBamboo's operations. Firewalls, NAS appliances, and storage sync systems are invisible to most endpoint security stacks. The actor exploits that blind spot and stays quiet for as long as needed.

The post-remediation return using stolen admin credentials is a reminder that credential rotation is not optional after an incident. VerdantBamboo adapted immediately after the initial fix and re-entered through a different path.

---

Cross-Reference: MSP compromise vector and European infrastructure targeting relevant to DE-Threats monitoring — unnamed victim assessed as Western enterprise, pfSense deployment common across European SMB and enterprise MSP environments.

---

# Incident #004 — June 9, 2026

**Target:** Red Hat developers and CI/CD pipelines using @redhat-cloud-services npm packages; downstream cloud environments (GCP, Azure, AWS, Kubernetes, Vault)

**Sector:** Developer Tools / Open Source / Cloud Infrastructure / CI/CD

**Threat Actor:% Miasma campaign — linked to Mini Shai-Hulud/TeamPCP (aka Replicating Marauder, TGR-CRI-1135, UNC6780) tooling — open-sourced attack tools, attribution complicated

**Origin:** Unknown — Russian-locale evasion pattern consistent with prior Shai-Hulud waves — possible Russian-ecosystem origin, unconfirmed

**Source:** The Hacker News — June 1, 2026 | Socket | Aikido Security | JFrog | Wiz | ReversingLabs | OX Security | CISA

**Attack Type:** npm Supply Chain Compromise → Credential-Stealing Worm → CI/CD Poisoning → Cloud Identity Theft

**Labels:** Miasma | Mini Shai-Hulud | TeamPCP | Red Hat | npm | Supply Chain | Credential Theft | CI/CD | GitHub Actions | Worm | Russian Locale Evasion | Sigstore Abuse | Claude Code | VS Code | GCP | Azure | Kubernetes

---

## Analysis

On June 1, 2026, researchers across Socket, Aikido Security, JFrog, Wiz, ReversingLabs, OX Security, and others disclosed Miasma — a new Mini Shai-Hulud supply chain campaign that compromised multiple @redhat-cloud-services npm packages with a credential-stealing, self-propagating worm. Evidence suggests a Red Hat employee's GitHub account was patient zero — compromised credentials and session cookies appearing in infostealer logs on April 13 and May 15, 2026 according to Whiteintel. The attacker used this access to push malicious orphan commits to two RedHatInsights repositories, bypassing code review. First Miasma commit containing "The Spreading Blight" string appeared May 29, 2026.

The malicious packages contain an obfuscated preinstall hook that harvests GitHub Actions secrets, npm tokens, cloud credentials (GCP, Azure, AWS), Kubernetes and Vault material, SSH keys, and Git credentials. Collected data is encrypted and exfiltrated to a spoofed Anthropic API endpoint (`api.anthropic[.]com:443/v1/api`) with GitHub as fallback — stolen tokens are committed back to GitHub repositories with a message threatening system destruction if the token is invalidated. For npm propagation, the malware exchanges OIDC tokens, repackages tarballs, and signs artifacts through Sigstore — making malicious packages appear legitimately signed. Persistence is established by injecting a SessionStart hook into Claude Code settings (`~/.claude/settings.json`) and a `tasks.json` with `"runOn": "folderOpen"` for VS Code projects — ensuring re-execution on every IDE session.

Wiz noted a significant evolution in this variant: new collectors for GCP and Azure cloud identities targeting all identities the infected machine has access to, not just secrets. Each infection generates a uniquely encrypted payload, making detection and version tracking significantly harder than prior waves. The malware checks for CrowdStrike, SentinelOne, Carbon Black, and StepSecurity Harden-Runner before executing, and avoids execution on Russian-locale systems — a pattern consistent across all Mini Shai-Hulud waves.

---

## Key Technical Indicators:
- **Affected packages:** @redhat-cloud-services/vulnerabilities-client, tsc-transform-imports, topological-inventory-client, sources-client, rule-components, remediations-client, rbac-client
- **Patient zero:** Red Hat employee GitHub account — credentials in infostealer logs April 13 and May 15, 2026
- **Initial delivery:** malicious orphan commits to RedHatInsights repositories — code review bypassed
- **Harvest targets:** GitHub Actions secrets, npm tokens, GCP/Azure/AWS credentials, Kubernetes/Vault material, SSH keys, Git credentials
- **Exfiltration:** encrypted payload to spoofed `api.anthropic[.]com:443/v1/api` — GitHub fallback
- **Commit threat:** `IfYouInvalidateThisTokenItWillNukeTheComputerOfTheOwner:<token>`
- **npm propagation:** OIDC token exchange, tarball repackaging (package-updated.tgz), Sigstore signing — appears as legitimate signed artifact
- **CI/CD escalation:** container bind-mounting host `/etc/sudoers.d` — grants CI runner passwordless sudo
- **Persistence:** Claude Code `~/.claude/settings.json` SessionStart hook + VS Code `tasks.json` `"runOn": "folderOpen"`
- **EDR evasion:** checks for CrowdStrike, SentinelOne, Carbon Black, StepSecurity Harden-Runner before executing
- **Russian locale:** skips execution on Russian-locale systems — consistent across all Shai-Hulud waves
- *Per-infection unique encrypted payload — version tracking significantly harder than prior waves*
- **First Miasma commit:** May 29, 2026

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1195.002 — Supply Chain Compromise: Compromise Software Supply Chain: Red Hat employee GitHub account compromised — malicious commits injected into @redhat-cloud-services npm packages bypassing code review
- **TA0002 — Execution**
  - T1059.004 — Command and Scripting Interpreter: Unix Shell: obfuscated preinstall hook executes credential harvesting logic at npm install time on developer machines
- **TA0003 — Persistence**
  - T1546 — Event Triggered Execution: SessionStart hook injected into Claude Code settings and VS Code tasks.json — malware re-executes on every IDE session open
- **TA0004 — Privilege Escalation**
  - T1611 — Escape to Host: CI/CD container launched with host `/etc/sudoers.d` bind-mount — grants passwordless sudo to CI runner
- **TA0005 — Defense Evasion**
  - T1497.001 — Virtualization/Sandbox Evasion: System Checks: EDR detection check for CrowdStrike, SentinelOne, Carbon Black, and StepSecurity before executing malicious payload
  - T1027 — Obfuscated Files or Information: per-infection unique encrypted payload — each instance distinct, complicating signature-based detection
  - T1553.002 — Subvert Trust Controls: Code Signing: malicious npm packages signed via Sigstore — appear as legitimately signed artifacts
- **TA0006 — Credential Access**
  - T1552.001 — Unsecured Credentials: Credentials in Files: GitHub Actions secrets, cloud credentials, SSH keys, Git credentials harvested from developer environments
  - T1528 — Steal Application Access Token: npm OIDC tokens exchanged and GitHub tokens extracted — used for downstream supply chain poisoning
- **TA0010 — Exfiltration**
  - T1567.001 — Exfiltration Over Web Service: Exfiltration to Code Repository: stolen credentials committed to attacker-controlled GitHub repositories labeled "Miasma: The Spreading Blight"

---

## Strategic Context

Miasma is the most technically evolved Mini Shai-Hulud wave to date. The shift from credential harvesting to full cloud identity collection — every GCP and Azure identity accessible from the infected machine — means this is no longer just about stealing secrets. It's about gaining persistent access to cloud environments that extend far beyond the developer machine itself.

The Russian locale evasion is the most analytically interesting detail for this repo. Every single Shai-Hulud wave skips Russian-locale systems. Six months of consistent activity, multiple campaigns, same evasion pattern. TeamPCP's tooling has been open-sourced, which makes attribution difficult, but the locale skip is a behavioral signature that persists regardless of who runs the tools. That's either a deliberate protection of a specific user base or a deeply embedded development default.

The Sigstore signing abuse is also worth tracking. Sigstore is a trusted artifact signing framework designed to improve supply chain integrity — using it to sign malicious packages is a direct attack on the trust model that defenders rely on to verify packages are legitimate.

---

## Russian Language Context

Последовательное избегание систем с русской локалью (posledovatelnoye izbeganie sistem s russkoy lokalyu) — consistent evasion of Russian-locale systems — across all Shai-Hulud waves remains the strongest behavioral indicator of российское происхождение (rossiyskoye proiskhozhdeniye) — Russian origin — despite unconfirmed attribution after шесть месяцев активности (shest mesyatsev aktivnosti) — six months of activity.

---

# Incident #005 — June 11, 2026

**Target:** Simulated heterogeneous networks — 33-host test environment emulating enterprise, IoT, and critical infrastructure deployments

**Sector:** Research / Proof-of-Concept / Cross-Sector (all internet-connected systems vulnerable)

**Threat Actor:** Autonomous AI-driven worm (theoretical) — no attribution, research demonstration

**Origin:** University of Toronto CleverHans Lab, Vector Institute, University of Cambridge, ServiceNow

**Source:** arXiv Preprint 2606.03811 — June 2, 2026 | The Hacker News — June 9, 2026

**Attack Type:** Self-Replicating AI Worm — Autonomous Vulnerability Discovery — Adaptive Exploit Generation — Runtime Payload Synthesis

**Labels:** AI Worm | Open-Weight LLM | Autonomous Agents | Self-Replicating | Adaptive Exploitation | Zero-Knowledge Network Reconnaissance | CVE Synthesis | Post-Training Reasoning | Heterogeneous Networks | A100 GPU | CleverHans | Nicolas Papernot | Proof-of-Concept | Threat Research

---

## Analysis

On June 2, 2026, researchers at the University of Toronto's CleverHans Lab published a proof-of-concept demonstrating the first fully autonomous, self-replicating AI worm powered by a locally-hosted open-weight LLM. The research, led by Professor Nicolas Papernot with collaborators from the Vector Institute, University of Cambridge, and ServiceNow, shows that attackers can now build adaptive worms using free, publicly available models without relying on commercial AI services or specialist tradecraft.

The prototype runs a 2025-era open-weight LLM on a single A100 GPU (80GB VRAM) or equivalent consumer hardware, and autonomously scans networks, identifies vulnerabilities, generates tailored exploit chains, and self-replicates — all without human intervention. In 15 isolated test runs on a 33-host network, the worm identified an average of 31.3 vulnerabilities, gained elevated access on 23.1 hosts (70%), and replicated to 20.4 hosts (62%) over seven days. Critically, the worm successfully exploited vulnerabilities disclosed in April and May 2026 — after the model's training cutoff — by ingesting public advisory information at runtime and converting it directly into working exploits. This demonstrates that adaptive worms can bypass the traditional "patch gap" defense by continuously testing new paths while defenders validate fixes.

The prototype targets three vulnerability classes: known CVEs (EternalBlue, SambaCry, PrintNightmare, Dirty Pipe), structural weaknesses (SQL injection, command injection, default credentials), and 0-day post-training disclosures. The researchers intentionally omitted implementation details that would enable misuse while providing enough technical depth for scientific scrutiny. Code access is restricted through a controlled process for qualified defensive researchers. The implications are direct: single-CVE patching breaks down when malware can inspect services, read fresh advisories, and generate a new attack path at runtime.

---

Key Technical Indicators:
- **Test environment:** 33 hosts (laptops, printers, cameras, IoT devices) — heterogeneous network simulation
- **LLM:** open-weight model published 2025 — runs on single A100 (80GB) or RTX PRO 6000 Blackwell
- **Vulnerability targeting:** CVEs (EternalBlue, SambaCry, PrintNightmare, Dirty Pipe), CWEs (SQL injection, command injection, default credentials), post-training disclosures (April–May 2026)
- **Exploit generation:** runtime synthesis from published advisories — no pre-built exploit database required
- **Average per-run metrics:** 31.3 vulnerabilities identified, 23.1 hosts (70%) compromised, 20.4 hosts (62%) replicated to over 7 days
- *No human intervention — fully autonomous propagation and exploitation*
- *No dependency on commercial AI APIs (OpenAI, Google, Anthropic) — uses open-weight model only*
- **Network knowledge:** zero knowledge of network topology required — discovers targets via scanning and exploitation
- **Self-replication:** autonomous clone to compromised hosts for reasoning operations
- **Research safeguards:** deliberate omission of implementation details, controlled code access, secure lab containment

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: autonomous exploitation of publicly disclosed but unpatched vulnerabilities (CVEs, CWEs) — no social engineering required
  - T1595.002 — Active Scanning: Network Service Discovery: worm autonomously scans networks to discover exposed services and map attack surface
- **TA0002 — Execution**
  - T1203 — Exploitation for Client Execution: vulnerabilities exploited for code execution on each discovered target
- **TA0003 — Persistence**
  - T1547 — Boot or Logon Autostart Execution: autonomous persistence mechanisms installed on compromised hosts
- **TA0004 — Privilege Escalation**
  - T1548 — Abuse Elevation Control Mechanism: exploitation of privilege escalation vulnerabilities to gain elevated access on targets
- **TA0005 — Defense Evasion**
  - T1027 — Obfuscated Files or Information: AI-generated exploit payloads dynamically generated per-target — no signature matches
  - T1222 — Windows File and Directory Permissions Modification: post-exploitation elevation and permission abuse
- **TA0007 — Discovery**
  - T1592.004 — Gather Victim Information: Software: LLM ingests published CVE data and advisory information to construct exploit chains
  - T1526 — Enumerate Cloud Resources: network scanning and service enumeration to identify attack surface
- **TA0042 — Resource Development**
  - T1587.001 — Develop Capabilities: Malware: autonomous AI-driven worm development and evolution without human coding

---

## Strategic Context

This research represents a fundamental shift in malware threat modeling. Traditional worms ship with a fixed exploit payload chosen at compile time. Patching those specific bugs stops the worm. An AI-driven worm doesn't work that way. It reads security advisories in real-time, generates new exploits on the fly, and tests them autonomously. Defenders can no longer assume that patching a CVE ends the threat — the worm is already testing the next path.

The fact that an open-weight model running on consumer-grade GPU is sufficient removes the dependency on cloud AI services that defenders have visibility into. There's no API call to OpenAI, Google, or Anthropic that would show up in logs. The entire reasoning engine runs locally on the compromised machine itself, making detection significantly harder.

The seven-day replication to 62% of a network is a proof-of-concept metric, but it underscores the speed and efficiency of autonomous worms relative to traditional manual exploitation. A human operator working 40 hours a week would take months to achieve what this worm did in seven days with zero human oversight.

---

## Russian Language Context

Автономный искусственный интеллект (avtonomnyy iskusstvennyy intellekt) — autonomous artificial intelligence — способный к саморепликации (sposobnyy k samoreplikatsii) — capable of self-replication — и генерации эксплойтов в реальном времени (i generatsii eksploytov v realnom vremeni) — and real-time exploit generation — представляет принципиальный сдвиг в парадигме киберугроз (predstavlyaet printsipial'nyy sdvig v paradigme kiberughoz) — represents a fundamental shift in the cyberthreat paradigm.

---

# Incident #006 — June 11, 2026

**Target:** Mongolian government entity — 12 confirmed infected systems; dozens of additional victims globally estimated via C2 traffic analysis

**Sector:** Government / Public Administration / Diplomatic

**Threat Actor:** GopherWhisper — previously undocumented China-aligned APT group

**Origin:** China — assessed state-sponsored, active since at least November 2023

**Source:** ESET Research — April 23, 2026 | Botconf 2026 | WeLiveSecurity White Paper

**Attack Type:** Multi-Backdoor Deployment → Legitimate Cloud Service C2 Abuse → Data Exfiltration

**Labels:** GopherWhisper | China-aligned | Go-Based Malware | LaxGopher | RatGopher | BoxOfFriends | Slack C2 | Discord C2 | Outlook/Graph API | CompactGopher | JabGopher | SSLORDoor | Mongolia | Government | Cloud Abuse | File.io

---

## Analysis

ESET Research disclosed a previously undocumented China-aligned APT group named GopherWhisper on April 23, 2026, after discovering it in January 2025. The group operates a purpose-built seven-component toolkit entirely written in Go, with one C++ fallback, and uses only legitimate cloud services for command-and-control: Slack, Discord, Microsoft 365 Outlook (via Microsoft Graph API), and file.io for data exfiltration. Initial discovery came from a Mongolian government entity with 12 infected systems; C2 traffic analysis from the attacker's Slack and Discord servers revealed dozens of additional victims globally, though their geolocation and verticals remain unconfirmed.

GopherWhisper's toolkit is architecturally redundant — three separate backdoors operating three separate C2 channels ensure that losing access to any single platform does not sever operator control. LaxGopher retrieves commands from private Slack servers and executes via cmd.exe with payload delivery capability. RatGopher operates identically over private Discord servers with bidirectional command execution and result reporting. BoxOfFriends exploits the Microsoft 365 Outlook REST API (Microsoft Graph) to establish C2 via draft emails — a technique that blends malicious traffic structurally with normal enterprise cloud communications. JabGopher acts as an injector, executing LaxGopher disguised as whisper.dll via legitimate svchost.exe process injection to evade endpoint detection. CompactGopher is a file collection utility that filters by extension (.doc, .docx, .jpg, .xls, .xlsx, .txt, .pdf, .ppt, .pptx), compresses into ZIP, encrypts with AES-CFB-128, and exfiltrates to file.io. SSLORDoor is a C++ fallback backdoor for additional resilience.

Initial access vector remains unknown as of the ESET disclosure. Post-compromise activity shows systematic data collection focused on official documents and communications. The group's seven-month initial detection lag (January discovery of November 2023 activity) suggests sophisticated evasion and long-term undetected access across the target environment.

---

## Key Technical Indicators:
- **Toolkit:** seven-component purpose-built arsenal — LaxGopher, RatGopher, BoxOfFriends (all Go), JabGopher (Go injector), CompactGopher (Go exfiltration), FriendDelivery (DLL loader), SSLORDoor (C++ fallback)
- **C2 channels:** private Slack servers, private Discord servers, Microsoft 365 Outlook draft emails (Microsoft Graph API), file.io file-sharing service
- **Confirmed victim:** Mongolian government entity — 12 infected systems
- **Estimated additional victims:** dozens globally (geolocation and verticals unknown)
- **Discovery date:** January 2025 (first LaxGopher detection)
- **Active since:** at least November 2023 (7-month undetected dwell)
- **C2 traffic analysis:** ESET analyzed attacker-operated Slack and Discord channels for operational insights
- **File extensions targeted:** .doc, .docx, .jpg, .xls, .xlsx, .txt, .pdf, .ppt, .pptx
- **Encryption:** AES-CFB-128 for compressed file archives before exfiltration
- **Process injection:** LaxGopher injected as whisper.dll into svchost.exe
- **Attribution:** ESET — high confidence China-aligned, active since at least 2023

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566 — Phishing: initial access vector unknown — likely phishing or credential compromise given government targeting and systematic post-compromise collection
- **TA0002 — Execution**
  - T1059.006 — Command and Scripting Interpreter: Python: command execution via cmd.exe through Slack and Discord C2 channels
  - T1106 — Native API: JabGopher uses Windows API for svchost.exe process injection
- **TA0003 — Persistence**
  - T1547 — Boot or Logon Autostart Execution: injected whisper.dll persists via svchost.exe integration
  - T1547.010 — Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder: persistence mechanisms installed in compromised systems
- **TA0005 — Defense Evasion**
  - T1578.003 — Modify Cloud Compute Infrastructure: legitimate cloud service abuse (Slack, Discord, Outlook) makes C2 traffic structurally indistinguishable from normal enterprise communications
  - T1036 — Masquerading: LaxGopher disguised as whisper.dll within legitimate processes
  - T1574.002 — Hijack Execution Flow: DLL Side-Loading: FriendDelivery loader used for malware delivery
- **TA0009 — Collection**
  - T1005 — Data from Local System: CompactGopher filters and collects files matching target extensions from compromised hosts
  - T1123 — Audio Capture / T1113 — Screen Capture: post-compromise surveillance operations documented via C2 traffic analysis
- **TA0010 — Exfiltration**
  - T1567.002 — Exfiltration Over Web Service: Exfiltration to Cloud Storage: CompactGopher exfiltrates to file.io
  - T1020 — Automated Exfiltration: automated file collection and encryption before exfiltration
- **TA0011 — Command and Control**
  - T1071 — Application Layer Protocol: Slack, Discord, and Microsoft Graph API used as legitimate C2 channels blending into normal traffic
  - T1102.003 — Web Service: Dead Drop Resolver: file.io used as dead-drop for data exfiltration

---

## Strategic Context

GopherWhisper represents a mature evolution in C2 tradecraft. Rather than building custom command-and-control infrastructure, the group simply abused legitimate services that every corporate firewall is configured to allow. Slack, Discord, and Outlook traffic cannot be blocked without disrupting legitimate business operations — making them perfect cover for attacker communications. The architectural redundancy of three separate backdoors and three separate C2 channels means that even if defenders detect and block one channel, the other two remain operational.

The seven-month undetected dwell between November 2023 and January 2025 suggests either very careful operational security or slow, deliberate exploitation focused on depth of access rather than breadth. Post-compromise activity centered on government documents and communications — consistent with Chinese intelligence collection priorities against neighboring states and regional powers.

The Go-based toolkit is deliberate. Go binaries are harder to reverse-engineer than .NET, and the language choice signals a group with development resources and long-term infrastructure planning.

---

# Incident #007 — June 11, 2026

**Target:** Universities across the United States — education sector organizations using Oracle PeopleSoft Enterprise for administrative operations

**Sector:** Education / Higher Education / Administration

**Threat Actor:** ShinyHunters (aka UNC6240 per Mandiant) — extortion crew, unattributed origin

**Source:** The Hacker News — June 11, 2026 | Google Mandiant (Charles Carmakal) | Oracle Security Alert CVE-2026-35273

**Attack Type:** Zero-Day RCE Exploitation → Data Theft → Extortion

**Labels:** ShinyHunters | UNC6240 | Oracle PeopleSoft | CVE-2026-35273 | CVSS 10.0 | Zero-Day | RCE | Unauthenticated | Universities | Education | Data Breach | Extortion | PSEMHUB | Environment Management

---

## Analysis

ShinyHunters exploited an unpatched zero-day in Oracle PeopleSoft (CVE-2026-35273, CVSS 9.8) between May 27 and June 9, 2026, targeting universities for data theft and extortion. Google Mandiant attributes the activity to UNC6240. The vulnerability is a remote code execution flaw in the PeopleSoft Enterprise PeopleTools Environment Management component, requiring no authentication or user interaction — just network access over HTTP to the PSEMHUB endpoint. Sixty-eight percent of the 100+ organizations Mandiant notified were higher education institutions, predominantly in the United States.

The University of Nottingham is a confirmed victim with approximately 455,000 unique email addresses exposed, including current students and alumni with names, addresses, phone numbers, passport numbers, and disability and ethnicity information. ShinyHunters says victim outreach has only just started and most of their claims remain unpublished — suggesting many more organizations remain undisclosed. Attackers used a custom MeshCentral agent disguised as Microsoft Azure binaries, deployed lateral-movement scripts over SSH using hardcoded credential lists, and exfiltrated data compressed with zstd to the ShinyHunters leak site. Mandiant found staging infrastructure exposed on public IP addresses running Python's SimpleHTTP server, revealing .bash_history, custom agents, and lateral-movement scripts with clear attribution to the threat actor.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-35273 — Oracle PeopleSoft Enterprise PeopleTools — CVSS 9.8 — remote code execution
- **Vulnerable component:** PSEMHUB (PeopleSoft Environment Management Hub)
- **Attack vector:** HTTP POST requests to /PSEMHUB/hub and /PSIGW/HttpListeningConnector — no authentication required
- **Exploitation timeline:** May 27–June 9, 2026 — zero-day status entire duration
- **Oracle advisory:** June 10, 2026 — patch status unclear per support login requirements
- **Affected versions:** PeopleTools 8.61 and 8.62 — earlier unsupported versions likely vulnerable
- **Confirmed victims:** 100+ organizations notified by Mandiant, 68% higher education; University of Nottingham disclosed with 455,000 records
- **Attacker toolkit:** custom MeshCentral agents (Azure binary masquerading), lateral-movement script (victim_fanout.sh), SSH credential spraying, zstd data compression
- **C2 infrastructure:** azurenetfiles.net (spoofed Azure NetApp Files), public SimpleHTTP servers exposing staging files
- **Initial access vector:** unknown — likely web-facing PSEMHUB endpoints without proper access controls

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: CVE-2026-35273 exploited via HTTP requests to PSEMHUB endpoints — unauthenticated remote code execution
- **TA0002 — Execution**
  - T1190 — Exploitation leading to code execution on PeopleSoft servers
- **TA0003 — Persistence**
  - T1136.001 — Create Account: Local Account: rogue administrative accounts created via web shell post-exploitation
  - T1547 — Boot or Logon Autostart Execution: MeshCentral agent persistence mechanisms installed
- **TA0008 — Lateral Movement**
  - T1021.004 — Remote Services: SSH: lateral-movement script (victim_fanout.sh) performs SSH credential spraying against internal hosts from /etc/hosts
- **TA0009 — Collection**
  - T1005 — Data from Local System: student records, alumni data, personal identifiers collected from university administrative systems
- **TA0010 — Exfiltration**
  - T1020 — Automated Exfiltration: zstd compression and automated exfiltration to attacker-controlled servers
- **TA0040 — Impact**
  - T1486 — Data Encrypted for Impact: data encrypted and withheld for ransom (extortion objective)

---

## Strategic Context

ShinyHunters' typical modus operandi is vishing, stolen tokens, and weak access controls against SaaS and cloud platforms. A server-side zero-day in on-premises ERP software is a significant escalation for this group. The fact that they gained 13 days of exploitation time before Oracle's advisory suggests either a late discovery by Oracle or that the vulnerability sat patched longer than expected. Universities store vast amounts of sensitive data — student records, employee information, research data — making them high-value targets for both data theft and extortion. The open question is whether this was a one-off borrowed zero-day or the start of ShinyHunters moving into ERP exploitation as a new capability.

---

# Incident #008 — June 11, 2026

**Target:** Enterprise ServiceNow customer instances — primarily Australia platform release and older releases with certain configuration changes

**Sector:** Enterprise / IT Service Management / SaaS

**Threat Actor:** Unknown — initially assessed as malicious threat actors, later reassessed as likely security researchers or bug bounty participants

**Source:** The Hacker News — June 10, 2026 | BleepingComputer | ServiceNow Security Advisory (customer portal)

**Attack Type:** API Unauthenticated Access Exploitation → Data Querying → Credential/Token Exposure

**Labels:** ServiceNow | API Security | Unauthenticated Access | /api/now/related_list_edit/create | Misconfiguration | Bug Bounty | Security Researcher Activity | Data Access | June 2026 | Enterprise

---

## Analysis

ServiceNow disclosed a security incident on June 10, 2026 (after the fact) involving attackers exploiting an unauthenticated API endpoint to query customer instance data. The company detected anomalous activity in early June and applied a global security patch on June 5, 2026 to the /api/now/related_list_edit/create endpoint, which was misconfigured with `requires_authentication=false`. The flaw allowed unauthenticated HTTP requests to access sensitive data within customer instances, primarily affecting the Australia platform release and older releases where customers had made certain configuration changes. However, on June 10, ServiceNow updated its disclosure to state that the observed activity was likely tied to security researchers or customer-led research associated with bug bounty submissions rather than malicious threat actors.

ServiceNow received a confidential bug bounty submission describing the issue on April 22, 2026, but did not deploy the patch until June 5 — more than one month later — after anomalous activity began on June 3–4. The company confirmed evidence of successful queries against instance tables for a "subset of customers." Affected instances commonly store sensitive enterprise data including IT support tickets, employee records, internal documentation, asset inventories, security incident reports, configuration details, and notably credentials and API tokens shared during troubleshooting. The exploit activity was associated with IP address 51.159.98.241 and Guest user account logins. No malware or advanced exploitation tools were identified, and no link to known threat actor infrastructure has been established. The exact scope of exposed data remains undisclosed.

---

## Key Technical Indicators:
- **Vulnerable endpoint:** /api/now/related_list_edit/create — misconfigured with requires_authentication=false
- **Misconfiguration:** unauthenticated HTTP requests allowed to query instance data
- **Affected versions:** Australia platform release, older releases with specific configuration changes
- **Anomalous activity detected:** June 3–4, 2026
- **Security patch deployed:** June 5, 2026 (globally)
- **Bug bounty submission:** April 22, 2026 (similar issue reported — 44-day gap to patch)
- **Associated IP address:** 51.159.98.241
- **Associated account:** Guest user account
- **Reported querying activity:** successful queries of instance tables against subset of customers
- **CVE:** none assigned at time of disclosure
- **Data stored in affected instances:** IT tickets, employee records, documentation, asset lists, security reports, credentials, API tokens, configuration details

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: unauthenticated API endpoint exploited via HTTP requests — no credentials required
- **TA0005 — Defense Evasion**
  - T1078 — Valid Accounts: Guest user account exploited for unauthenticated access
- **TA0007 — Discovery**
  - T1526 — Enumerate Cloud Resources: instance tables queried via unauthenticated endpoint — reconnaissance of available data
- **TA0009 — Collection**
  - T1005 — Data from Local System: sensitive enterprise data queried from customer instances (support tickets, employee records, credentials)
- **TA0010 — Exfiltration**
  - T1030 — Data Transfer Size Limits: queries of instance tables implies data movement (scope unconfirmed)

---

## Strategic Context

The timeline raises operational security questions. ServiceNow received a bug bounty report on April 22, but did not patch until June 5 — more than a month later. Anomalous activity was detected on June 3–4, suggesting attackers discovered and exploited the flaw before the patch was even deployed. The June 10 reassessment that the activity was likely security researchers rather than malicious threat actors is notable but undercuts the severity claim. The fact that ServiceNow did not immediately assign a CVE identifier and buried the advisory behind its customer support portal suggests they initially approached it as a containment issue rather than a critical incident. The lack of detected malware or sophisticated tooling is consistent with either security researchers testing the flaw or opportunistic exploitation by less-skilled actors.

---

