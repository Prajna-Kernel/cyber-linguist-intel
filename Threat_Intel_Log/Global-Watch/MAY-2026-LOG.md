# 🛡️ Threat Intelligence Log — May 2026

> Active monitoring of cross-border cyber operations and state-sponsored activity.
> Focus: Non-regional incidents, wiper attacks, and AI-assisted threat tooling.
> Language access: English

---

# Incident #001 — May 1, 2026

**Target:** Software developers and Web3/cryptocurrency developers globally — npm and PyPI ecosystem users

**Sector:** Software Supply Chain / Cryptocurrency / Developer Infrastructure**Threat Actor:** Famous Chollima (aka Shifty Corsair) — DPRK state-sponsored; Contagious Interview campaign

**Origin:** North Korea — DPRK state-directed, high confidence

**Source:** The Hacker News — April 29, 2026 | ReversingLabs — April 29, 2026

**Attack Type:** Multi-Layer npm Supply Chain Attack — AI-Generated Malware / Fake Companies / RAT Deployment

**Labels:** DPRK | Famous Chollima | Shifty Corsair | PromptMink | Contagious Interview | Contagious Trader | graphalgo | OtterCookie | npm | PyPI | Crypto Theft | Fake LLC | AI-Generated Malware | Web3

---

## Analysis

Famous Chollima — the DPRK threat cluster behind the long-running Contagious Interview campaign — has evolved its supply chain operation significantly. Three distinct but overlapping campaigns are now running simultaneously: PromptMink, Contagious Trader, and graphalgo.

PromptMink is the newest and most technically interesting. A malicious package called @validate-sdk/v2 was inserted as a dependency into an autonomous trading agent project via a commit co-authored by Claude Opus — meaning the attacker used an AI coding assistant to write and commit the malicious code, bypassing the human review instinct that might catch suspicious additions. The malware uses a two-layer approach: first-layer packages look legitimate and import popular libraries with millions of downloads. A small number of their dependencies are second-layer malicious packages that do the actual stealing. If the second layer gets detected and removed, it's swapped out immediately. The payload targets crypto wallets and developer secrets, exfiltrating to a Vercel URL that Famous Chollima has used repeatedly across campaigns.

Contagious Trader runs a Matryoshka doll structure — a benign wrapper package downloads a malicious dependency which installs OtterCookie, a known Famous Chollima stealer. OtterCookie has now gained mouse and keyboard control capabilities via the @nut-tree-fork/nut-js package, upgrading it from an infostealer to a full interactive RAT. It uses socket.io-client for C2, screenshot-desktop for screen capture, and clipboardy for clipboard access — all legitimate packages repurposed as attack components.

Graphalgo is the social engineering arm. The actors create fake companies — Veltrix Capital, Blockmerce, Bridgers Finance — complete with GitHub organizations, LinkedIn profiles, and X accounts, and even registered Blockmerce as a real Florida LLC in August 2025. Developers are approached on job platforms with fake coding assessments. The assessment project contains a dependency to a malicious npm or PyPI package, or in recent iterations, a GitHub release artifact buried deep in the transitive dependency chain to avoid detection. The RAT deployed via graphalgo can execute system recon, file operations, and upload/download.

The most recent addition is a compromised axios package — one of the most downloaded npm packages in existence — attributed to UNC1069, a North Korea-linked cluster. A follow-on package csec-crypto-utils published afterward substituted the RAT dropper for a data stealer targeting AWS keys, GitHub tokens, and .npmrc files.

---

## Key Technical Indicators:
- **Campaign:** PromptMink — @validate-sdk/v2, AI-generated via Claude Opus commit co-authorship
- **Two-layer package structure:** benign first layer imports malicious second layer; second layer swapped on detection
- **Exfiltration:** ipfs-url-validator.vercel.app (Vercel URL — repeated Famous Chollima infrastructure)
- **Payload evolution:** JS obfuscated stealer → Node.js SEA (85MB) → NAPI-RS Rust pre-compiled add-ons
- **Campaign:** Contagious Trader — OtterCookie stealer via Matryoshka package chain
- **OtterCookie new capability:** mouse/keyboard control via @nut-tree-fork/nut-js
- **OtterCookie C2:** socket.io-client; screen capture: screenshot-desktop; clipboard: clipboardy
- **Campaign:** graphalgo — fake companies with GitHub orgs, LinkedIn, X, Florida LLC registration (Blockmerce, L25000392646)
- **Recent graphalgo shift:** malicious packages hosted as GitHub release artifacts instead of npm/PyPI to evade detection
- **graphalgo packages:** graph-dynamic, graphbase-js, graphlib-js
- **axios compromise:** attributed to UNC1069 / BlueNoroff — NukeSped RAT similarities confirmed by Hunt.io
- **Follow-on:** csec-crypto-utils — AWS keys, GitHub tokens, .npmrc exfil to csec-c2-server.onrender[.]com
- **Targets:** crypto wallets, .env files, SSH keys, developer secrets, source code projects (Rust payloads)
- *SSH backdoors deployed for persistent remote access in recent campaign iterations*

---

## MITRE ATT&CK Tactics:
- **TA0001 — Initial Access** — malicious npm/PyPI packages inserted as dependencies; fake job interview coding assessments used to deliver malicious GitHub projects; compromised axios package distributed to downstream users
- **TA0002 — Execution** — malicious payload executes on package import or install; OtterCookie deployed as second-stage; Rust-compiled payloads execute on Windows, Linux, macOS
- **TA0003 — Persistence** — SSH backdoors installed for persistent remote access; postinstall hooks injected into local npm packages to propagate on publish
- **TA0005 — Defense Evasion** — two-layer package structure hides malicious second layer behind legitimate first layer; AI-generated code used to evade human review; typosquatting and name mimicry; malicious deps buried in transitive dependency chain; GitHub release artifacts used instead of npm registry
- **TA0006 — Credential Access** — crypto wallets, .env files, SSH keys, AWS keys, GitHub tokens, .npmrc configs harvested from developer environments
- **TA0007 — Discovery** — system recon via graphalgo RAT — file enumeration, process listing, directory traversal
- **TA0009 — Collection** — entire source code projects exfiltrated via Rust payloads; screen capture, clipboard monitoring, keylogging via OtterCookie
- **TA0010 — Exfiltration** — data sent to Vercel-hosted C2; csec-c2-server.onrender[.]com; external attacker infrastructure
- **TA0011 — Command & Control** — socket.io-client used for OtterCookie C2; SSH backdoors for persistent access; JSON Keeper paste service used for second-stage payload delivery
- **TA0040 — Impact** — crypto wallet access and fund drainage; source code and IP theft from compromised developer environments

---

## Strategic Context

Three simultaneous campaigns, each targeting a different entry point — package registries, job platforms, and transitive dependencies — means Famous Chollima is operating at scale with deliberate specialization. PromptMink using Claude Opus as a co-author to insert malicious code is the most operationally significant detail here. AI coding assistants are now being weaponized not just to write attack tools but to insert them into real projects in ways that look legitimate to automated review systems.

The Florida LLC registration for Blockmerce is the clearest signal of how seriously this group treats operational preparation. Registering a real company, building out social profiles, maintaining GitHub activity for months before the attack — this is not opportunistic. It's a sustained deception infrastructure designed to withstand scrutiny from developers who do their homework before accepting a coding test.

The axios compromise via UNC1069 is separately significant. axios has hundreds of millions of downloads. Using it as an attack vector means the reach of a single compromise is theoretically unlimited. The shift from RAT dropper to credential stealer in the follow-on package suggests the group is adapting based on what defenders are looking for.

---

# Incident #002 — May 1, 2026

**Target:** Python developers using PyTorch Lightning — AI/ML framework users; Intercom npm package users

**Sector:** Software Supply Chain / AI/ML Infrastructure / Developer Infrastructure

**Threat Actor:** TeamPCP — Mini Shai-Hulud campaign; LAPSUS$ confirmed partner

**Origin:** Unknown — Russian locale skip present in prior waves

**Source:** The Hacker News — April 30, 2026 | Aikido Security | Socket | OX Security | StepSecurity

**Attack Type:** PyPI Package Compromise → CI/CD Pipeline Injection → Credential Theft + Worm-Like GitHub Propagation

**Labels:** TeamPCP | Mini Shai-Hulud | PyTorch Lightning | Intercom | PyPI | npm | CI/CD | GitHub Worm | LAPSUS$ | Credential Theft | Claude Code Impersonation | Supply Chain

---

## Analysis

TeamPCP — the group behind the Bitwarden CLI Shai-Hulud campaign documented in Global-Watch — has escalated. Two malicious versions of PyTorch Lightning (2.6.2 and 2.6.3) were published to PyPI on April 30, 2026, as part of the Mini Shai-Hulud campaign that also hit SAP npm packages earlier in the same week. PyTorch Lightning is an open-source ML framework with 31,100+ GitHub stars. PyPI quarantined the package after disclosure.

The attack chain is more sophisticated than prior Shai-Hulud waves. The malicious versions include a hidden _runtime directory containing a downloader and obfuscated JavaScript payload. When a developer imports the lightning module — not just installs it, but imports it — a Python script (start.py) fires automatically, downloads the Bun JavaScript runtime, and uses it to execute an 11MB obfuscated payload called router_runtime.js. The payload performs comprehensive credential harvesting identical to prior waves.

The GitHub token handling is the standout new capability. Harvested tokens are validated against api.github[.]com/user before being used to inject a worm-like payload into up to 50 branches across every repository the token can write to. The injected commits are authored using a hardcoded identity designed to impersonate Anthropic's Claude Code — a deliberate choice to blend into repos where AI coding assistants are actively used. The operation silently overwrites existing files without pre-checking content.

The npm propagation vector is equally notable. The malware modifies the developer's local npm packages by injecting a postinstall hook, bumping the patch version, and repacking the .tgz tarballs. If the developer publishes from their local environment — which many do without auditing — the tampered package lands on npm and propagates to downstream users.

In the same campaign wave, intercom-client npm version 7.0.4 was compromised using the same preinstall hook mechanism. Socket confirmed technical overlap with the SAP CAP campaign, Bitwarden, Telnyx, LiteLLM, and Aqua Security Trivy — all prior TeamPCP targets. TeamPCP has now launched a dark web onion site after its X account was suspended, and publicly claimed LAPSUS$ as a partner and participant throughout the operation.

---

## Key Technical Indicators:
- **Compromised packages:** PyTorch Lightning v2.6.2 and v2.6.3 (PyPI); intercom-client v7.0.4 (npm)
- **Hidden directory:** _runtime — contains downloader and obfuscated JS payload
- **Execution trigger:** fires on module import — not just install
- **Payload:** router_runtime.js (11MB obfuscated) — executed via Bun JavaScript runtime downloaded by start.py
- **GitHub token weaponization:** validated against api.github[.]com/user; worm-like injection into up to 50 branches per repo
- *Injected commits impersonate Anthropic Claude Code identity*
- **npm propagation:** postinstall hook injected into local packages → patch version bumped → .tgz repacked → published to npm by unsuspecting developer
- *intercom-client v7.0.4 compromised — same preinstall hook mechanism as SAP CAP campaign*
- *TeamPCP dark web onion site launched after X suspension*
- *LAPSUS$ publicly confirmed as campaign partner by TeamPCP*
- **Campaign scope overlap:** Bitwarden, Telnyx, LiteLLM, Aqua Security Trivy, SAP CAP, Checkmarx, Intercom
- *PyPI quarantined affected versions; downgrade to v2.6.1*

---

## MITRE ATT&CK Tactics:
- **TA0001 — Initial Access** — compromised PyTorch Lightning versions published to PyPI; intercom-client npm package compromised via same campaign
- **TA0002 — Execution** — payload fires automatically on module import; Bun runtime downloaded and used to execute obfuscated JS payload
- **TA0003 — Persistence** — worm-like payload injected into up to 50 GitHub repo branches; postinstall hooks injected into local npm packages for downstream propagation
- **TA0005 — Defense Evasion** — payload hidden in _runtime directory; commits impersonate Claude Code identity to blend into AI-assisted repos; file overwrites performed silently with no pre-check
- **TA0006 — Credential Access** — comprehensive developer credential harvesting — GitHub tokens, npm tokens, SSH keys, cloud secrets, CI/CD variables
- **TA0009 — Collection** — developer environment sweep; GitHub repo contents accessible via validated tokens
- **TA0010 — Exfiltration** — credentials and repo data exfiltrated to TeamPCP infrastructure
- **TA0040 — Impact** — worm propagation across developer repos; downstream npm users exposed via repacked tampered packages; LAPSUS$ involvement expands potential impact scope

---

## Strategic Context

TeamPCP is no longer a one-off supply chain actor — they're running a sustained campaign across multiple ecosystems simultaneously. Bitwarden, LiteLLM, Telnyx, SAP, Aqua Security Trivy, Intercom, and now PyTorch Lightning. Each hit expands the credential pool and the propagation surface.

The Claude Code impersonation detail is the most operationally significant part of this entry. Poisoned commits authored under an AI coding assistant identity are designed to be invisible in repos where Claude Code is already being used legitimately. Developers see a Claude Code commit and don't question it. That's a deliberate trust exploitation built around how modern development workflows actually look in 2026.

The LAPSUS$ partnership confirmation changes the threat profile. LAPSUS$ has a documented history of high-impact breaches against major tech companies — Microsoft, Okta, Nvidia, Rockstar Games. TeamPCP with LAPSUS$ involvement is not the same threat as TeamPCP alone.

The worm propagation mechanism is what makes this entry different from a standard supply chain compromise. One infected developer publishing from a local environment becomes a distribution node. The attack self-replicates through normal developer behavior without any additional attacker action required.

---

# Incident #003 — May 1, 2026

**Target:** cPanel and WHM deployments globally — web hosting providers, shared hosting environments, WP Squared WordPress hosting

**Sector:** Web Hosting Infrastructure / Shared Hosting / Internet Infrastructure

**Threat Actor:** Unknown — opportunistic exploitation confirmed since February 23, 2026

**Origin:** Unattributed

**Source:** BleepingComputer — April 30, 2026 | Rapid7 | watchTowr | Help Net Security | SecurityWeek

**Attack Type:** Authentication Bypass via CRLF Injection → Unauthenticated Root Access — Zero-Day Exploited Since February

**Labels:** CVE-2026-41940 | CVSS 9.8 | cPanel | WHM | WP Squared | Auth Bypass | CRLF Injection | Zero-Day | Active Exploitation | PoC Available | Web Hosting | 1.5M Exposed

---

## Analysis

CVE-2026-41940 is a CVSS 9.8 authentication bypass in cPanel, WHM, and WP Squared — the web hosting control panels running on an estimated 1.5 to 2 million internet-exposed servers. The root cause is CRLF injection in the login and session loading processes. Before authentication occurs, cpsrvd (the cPanel service daemon) writes a new session file to disk using user-controlled input from the Authorization header without proper sanitization. Attackers can inject arbitrary values into that session file, remove a hex value that triggers encryption, and pass plaintext make-me-root commands through as trusted code — bypassing authentication entirely and gaining root-level WHM access.

cPanel disclosed and patched on April 28, 2026 — but KnownHost, a hosting provider, confirmed exploitation attempts as early as February 23, meaning the vulnerability was used as a zero-day for roughly two months before the patch. cPanel was reportedly notified approximately two weeks before the public advisory and initially responded that nothing was wrong, delaying the patch and leaving the hosting industry exposed without warning. watchTowr published a technical analysis and Detection Artifact Generator script. A public PoC now exists, dramatically lowering the exploitation bar.

Root-level WHM access means an attacker can read every customer hosting account on a shared server, modify files and databases, create backdoor accounts, install malware, steal credentials, and pivot into customer networks. As watchTowr put it — it's the keys to every apartment in the kingdom, and the kingdom is the internet. Multiple major hosting providers including Namecheap, KnownHost, HostPapa, and InMotion blocked cPanel ports 2083 and 2087 as emergency mitigations while patches were deployed.

The vulnerability affects all cPanel and WHM versions after v11.40 and WP Squared v136.1.7 and earlier. cPanel initially did not communicate the existence of the vulnerability to hosting providers while working on a fix — a disclosure process failure that left providers unable to take protective action.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-41940, CVSS 9.8
- **Vulnerability class:** CRLF injection in cPanel/WHM login and session loading — auth bypass via session file manipulation
- **Root cause:** cpsrvd writes user-controlled Authorization header input into session file before authentication without sanitization
- **Exploit:** remove hex value to bypass encryption → inject plaintext commands → gain root WHM access
- **Affected:** all cPanel/WHM versions after v11.40; WP Squared v136.1.7 and earlier
- **Active exploitation confirmed:** February 23, 2026 — two months before patch
- **Disclosure:** cPanel notified ~2 weeks before April 28 advisory; initially responded "nothing wrong"
- **Exposed instances:** ~1.5–2 million internet-facing cPanel servers (Shodan/Eye Security)
- **Vulnerable ports:** 2083, 2087, 2095, 2096
- **PoC:** watchTowr Detection Artifact Generator published April 29, 2026
- **Patched versions:** 11.86.0.41, 11.110.0.97, 11.118.0.63, 11.126.0.54, 11.130.0.19, 11.132.0.29, 11.136.0.5, 11.134.0.20; WP Squared 136.1.7
- **Emergency mitigations:** block ports 2083, 2087, 2095, 2096; stop cpsrvd and cpdavd services
- **Known exploitation impact:** confirmed as "let me see if this works" probing at KnownHost — no confirmed destructive activity yet

---

## MITRE ATT&CK Tactics:
- **TA0001 — Initial Access** — unauthenticated CRLF injection via crafted Authorization header bypasses cPanel/WHM login flow; root-level access achieved without valid credentials
- **TA0002 — Execution** — plaintext commands passed through session file as trusted code after hex value removal bypasses encryption
- **TA0004 — Privilege Escalation** — authentication bypass directly yields root-level WHM administrative access
- **TA0005 — Defense Evasion** — exploited as zero-day for ~2 months before disclosure; no public PoC required for initial exploitation; cPanel delayed notification to hosting providers
- **TA0006 — Credential Access** — root WHM access enables reading of all customer credentials, databases, and configuration files across shared hosting environments
- **TA0008 — Lateral Movement** — root access to shared hosting server enables pivot into all customer hosting accounts and potentially customer networks
- **TA0040 — Impact** — full server takeover; all hosted websites and databases at risk; backdoor installation, malware deployment, and data theft possible across all accounts on compromised host

---

## Strategic Context

1.5 to 2 million exposed instances, CVSS 9.8, zero-day exploited for two months, public PoC now available. This is the definition of a mass exploitation event waiting to happen. The current confirmed activity is probe-level — attackers testing the waters — but with a PoC public and the flaw this straightforward, that will not last.

The disclosure process failure is worth flagging separately from the vulnerability itself. cPanel was notified two weeks before going public and initially dismissed the report. Hosting providers had no warning and couldn't take action while the patch was being developed. For a vulnerability of this severity affecting this much internet infrastructure, that's an institutional failure that amplified risk across the entire hosting industry.

Any unpatched cPanel or WHM server should be treated as potentially already compromised given the two-month zero-day window. Patching is the floor — running cPanel's detection script and watchTowr's artifact generator is the actual minimum.

---

# Incident #004 — May 2, 2026

**Target:** Government and defense sectors across South, East, and Southeast Asia; one NATO member (Poland); journalists and civil society including Uyghur, Tibetan, Taiwanese, and Hong Kong diaspora activists; ICIJ and international journalists

**Sector:** Government / Defense / Civil Society / Journalism

**Threat Actor:** SHADOW-EARTH-053 — China-aligned espionage cluster; GLITTER CARP and SEQUIN CARP — China-affiliated phishing clusters (Citizen Lab)

**Origin:** China — assessed with medium-high confidence; commercial contractors hired by Chinese state suspected

**Source:** The Hacker News — May 1, 2026 | Trend Micro (Daniel Lunghi, Lucas Silva) | Citizen Lab

**Attack Type:** N-day Exploitation → Web Shell Deployment → ShadowPad Backdoor; Phishing → Credential Harvesting → OAuth Token Theft

**Labels:** SHADOW-EARTH-053 | GLITTER CARP | SEQUIN CARP | ShadowPad | Godzilla | Noodle RAT | ProxyLogon | IIS | DLL Sideloading | Mimikatz | China-Nexus | Espionage | Journalists | Activists | NATO | India | Pakistan | Poland

---

## Analysis

Trend Micro disclosed SHADOW-EARTH-053, a China-aligned espionage cluster active since at least December 2024, running sustained intrusion campaigns against government and defense targets across Pakistan, Thailand, Malaysia, India, Myanmar, Sri Lanka, and Taiwan — plus Poland as the lone NATO member in its victimology. The group shares network overlap with CL-STA-0049, Earth Alux, and REF7707, suggesting a shared contractor or infrastructure pool within China's broader cyber espionage ecosystem.

The entry vector is N-day exploitation of internet-facing Microsoft Exchange and IIS servers — ProxyLogon being the primary chain used. Once inside, Godzilla web shells are deployed for persistent remote access and command execution. From there, the group stages ShadowPad implants via DLL sideloading of legitimate signed executables, a technique that abuses trusted binaries to load malicious DLLs without triggering standard detection. AnyDesk is used as a delivery vehicle for the ShadowPad backdoor. In at least one case, React2Shell (CVE-2025-55182) was used to deploy a Linux version of Noodle RAT. GTIG linked this specific chain to UNC6595.

Lateral movement uses a custom RDP launcher and Sharp-SMBExec. Privilege escalation uses Mimikatz. Open-source tunneling tools — IOX, GOST, and Wstunnel — handle traffic obfuscation. RingQ packs malicious binaries to evade detection.

Citizen Lab simultaneously disclosed GLITTER CARP and SEQUIN CARP. GLITTER CARP targets ICIJ and the Taiwanese semiconductor industry, using 1x1 tracking pixels in phishing emails to fingerprint recipients before deploying credential harvesting pages. SEQUIN CARP targets specific journalists covering topics of Chinese government interest, sharing infrastructure with Volexity's UTA0388 and Trend Micro's TAOTH. Both clusters use AiTM phishing kits and OAuth token theft as final access objectives. Citizen Lab assesses with medium confidence that commercial entities hired by the Chinese state are behind both clusters.

## Key Technical Indicators:
- **Threat cluster:** SHADOW-EARTH-053 — active since December 2024; overlaps with CL-STA-0049, Earth Alux, REF7707
- **Entry vector:** N-day exploitation of Exchange and IIS — ProxyLogon chain primary
- **Web shell:** Godzilla — persistent remote access and command execution
- **Backdoor:** ShadowPad — deployed via DLL sideloading of legitimate signed executables; delivered via AnyDesk
- **Secondary backdoor:** Noodle RAT (Linux) — deployed via CVE-2025-55182 (React2Shell); linked to UNC6595
- **Privilege escalation:** Mimikatz
- **Lateral movement:** custom RDP launcher; Sharp-SMBExec (C# SMBExec implementation)
- **Tunneling:** IOX, GO Simple Tunnel (GOST), Wstunnel
- **Evasion:** RingQ used to pack malicious binaries
- **Targets:** Pakistan, Thailand, Malaysia, India, Myanmar, Sri Lanka, Taiwan, Poland (NATO)
- **SHADOW-EARTH-054 overlap:** Malaysia, Sri Lanka, Myanmar targets compromised by both clusters
- **GLITTER CARP:** targets ICIJ, Taiwanese semiconductor industry; 1x1 tracking pixels; AiTM phishing kit; linked to UNK_SparkyCarp
- **SEQUIN CARP:** targets international journalists; overlaps with UTA0388 and TAOTH
- **End goal:** email account access via credential harvesting, phishing pages, or OAuth token social engineering

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: ProxyLogon chain exploited against internet-facing Exchange and IIS servers
  - T1566 — Phishing: GLITTER CARP and SEQUIN CARP deliver credential harvesting pages and OAuth token social engineering via email

- **TA0002 — Execution**
  - T1059 — Command and Scripting Interpreter: Godzilla web shell executes commands on compromised servers
  - T1072 — Software Deployment Tools: Sharp-SMBExec used for remote execution during lateral movement

- **TA0003 — Persistence**
  - T1505.003 — Server Software Component: Web Shell: Godzilla web shell maintains persistent remote access on compromised Exchange/IIS servers
  - T1543 — Create or Modify System Process: ShadowPad deployed as long-term persistent backdoor implant

- **TA0004 — Privilege Escalation**
  - T1068 — Exploitation for Privilege Escalation: Mimikatz used to extract credentials and escalate privileges on compromised systems

- **TA0005 — Defense Evasion**
  - T1574.002 — Hijack Execution Flow: DLL Side-Loading: ShadowPad loaded via DLL sideloading of legitimate signed executables
  - T1027 — Obfuscated Files or Information: RingQ used to pack and obfuscate malicious binaries
  - T1072 — Software Deployment Tools: AnyDesk used as legitimate delivery vehicle for ShadowPad
  - T1090 — Proxy: IOX, GOST, Wstunnel tunnel C2 traffic through legitimate-looking connections

- **TA0006 — Credential Access**
  - T1003 — OS Credential Dumping: Mimikatz extracts credentials from compromised systems
  - T1539 — Steal Web Session Cookie: GLITTER CARP and SEQUIN CARP harvest OAuth tokens via AiTM phishing kits

- **TA0007 — Discovery**
  - T1083 — File and Directory Discovery: reconnaissance conducted via Godzilla web shell before ShadowPad staging
  - T1598 — Phishing for Information: 1x1 tracking pixels used to fingerprint and confirm target email opens

- **TA0008 — Lateral Movement**
  - T1021.001 — Remote Services: Remote Desktop Protocol: custom RDP launcher used for lateral movement
  - T1021.002 — Remote Services: SMB/Windows Admin Shares: Sharp-SMBExec moves between internal systems via SMB

- **TA0009 — Collection**
  - T1114 — Email Collection: journalist and activist email accounts accessed via harvested credentials and OAuth tokens
  - T1005 — Data from Local System: government and defense data collected via ShadowPad backdoor

- **TA0011 — Command & Control**
  - T1071 — Application Layer Protocol: ShadowPad C2 communication via application layer protocols
  - T1090 — Proxy: IOX, GOST, Wstunnel obfuscate C2 traffic through tunneling

## Strategic Context

India is explicitly in SHADOW-EARTH-053's targeting list. Pakistan too. A China-aligned APT running sustained operations against both simultaneously — in the context of ongoing India-Pakistan tensions — means this group is positioned to collect intelligence on both sides of a live conflict. That's a significant strategic advantage for Beijing regardless of which direction the situation develops.

The Poland angle is notable. One NATO member inside an otherwise Asia-Pacific campaign suggests SHADOW-EARTH-053 is not purely a regional actor — it's tracking European entities with connections to the Asian targets, likely diplomatic or defense cooperation threads.

The Citizen Lab disclosure of GLITTER CARP and SEQUIN CARP adds the civil society dimension. Targeting Uyghur, Tibetan, Taiwanese, and Hong Kong diaspora activists alongside investigative journalists is a transnational repression operation running parallel to the state espionage campaign. These aren't separate priorities — they're the same intelligence collection doctrine applied to different target categories.

---

# ⚠️ Cross-Sector Alert: Russia — Parallel Government & Journalist Espionage Operations
**Status:** Active — May 2026   
**Target:** German government officials including cabinet members — Signal phishing campaign   
**Note:** Russia and China running simultaneous espionage operations against government officials and journalists in overlapping timeframes. Different actors, different methods, same target categories.

👉 **[Read Full Entry in RU-Threats](../RU-Threats/MAY-2026-LOG.md#incident-001)**
