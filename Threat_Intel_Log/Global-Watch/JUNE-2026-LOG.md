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

Incident #003 — June 9, 2026

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
