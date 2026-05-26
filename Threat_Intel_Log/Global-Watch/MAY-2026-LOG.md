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

---

# Incident #005 — May 4, 2026

**Target:** Mongolian government entities; Indian banks

**Sector:** Government / Financial / Banking

**Threat Actor:** GopherWhisper — newly identified China-aligned APT

**Origin:** China — assessed

**Source:** CYFIRMA Weekly Intelligence — May 1, 2026

**Attack Type:** Spearphishing → Go-based Malware → C2 via Legitimate Collaboration Platforms

**Labels** GopherWhisper | China-Nexus | Mongolia | India | Go Malware | Discord | Slack | Outlook | file.io | Espionage | Banking | Government

---

## Analysis

CYFIRMA researchers published a report on GopherWhisper, a newly identified China-aligned APT targeting Mongolian government entities and Indian banks. The group uses malware written in Go — a language increasingly favored by threat actors for its cross-platform capability, smaller detection footprint, and ease of static compilation. What makes GopherWhisper operationally notable is its C2 architecture — the group abuses Discord, Slack, Microsoft 365 Outlook, and file.io as command-and-control and data exfiltration channels.

Using legitimate collaboration platforms as C2 is a deliberate evasion strategy. Traffic to Discord, Slack, and Outlook blends seamlessly into normal enterprise and government network activity — standard reputation-based filtering won't flag it. Researchers were able to extract thousands of C2 messages from these services, giving unusual insight into the group's operational tempo and targeting patterns. The volume of extracted messages suggests GopherWhisper has been running sustained operations, not opportunistic hits.

Targeting Mongolian government entities and Indian banks simultaneously reflects a China intelligence collection priority — Mongolia sits on China's northern border and maintains historically complex relationships with both China and Russia, making it a consistent target for Beijing's intelligence apparatus. Indian banking infrastructure is a high-value target for both financial intelligence and potential pre-positioning.

---

## Key Technical Indicators:
- **Threat actor:** GopherWhisper — newly identified, China-aligned
- **Malware language:** Go — cross-platform, static compilation, smaller AV footprint
- **C2 channels:** Discord, Slack, Microsoft 365 Outlook, file.io — legitimate platform abuse
- *Thousands of C2 messages extracted by researchers — sustained operational tempo confirmed*
- **Targets:** Mongolian government entities, Indian banks
- **Initial access vector:** spearphishing — consistent with China-nexus APT doctrine

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566 — Phishing: spearphishing used to deliver Go-based malware to Mongolian government and Indian banking targets

- **TA0002 — Execution**
  - T1059 — Command and Scripting Interpreter: Go-based malware executes post-delivery on compromised systems

- **TA0005 — Defense Evasion**
  - T1102 — Web Service: legitimate collaboration platforms (Discord, Slack, Outlook, file.io) used as C2 channels to blend into normal network traffic

- **TA0009 — Collection**
  - T1114 — Email Collection: government and banking data collected from compromised environments
  - T1005 — Data from Local System: sensitive data harvested from Mongolian government and Indian bank systems

- **TA0010 — Exfiltration**
  - T1567 — Exfiltration Over Web Service: data exfiltrated via file.io and collaboration platform channels

- **TA0011 — Command & Control**
  - T1102 — Web Service: Discord, Slack, Microsoft 365 Outlook used as C2 infrastructure
  - T1071 — Application Layer Protocol: C2 communication via application layer protocols on legitimate platforms

---

## Strategic Context

India is directly in GopherWhisper's targeting scope — Indian banks specifically. Combined with SHADOW-EARTH-053 (#004) targeting Indian government and defense entities, this is the second China-aligned APT documented in this log this month with confirmed Indian targets. Two separate Chinese APT clusters, different tooling, overlapping target geography — that's not coincidence, it's a consistent intelligence collection priority against India.

The platform abuse model is worth watching. GopherWhisper is not the first group to use Discord or Slack for C2 but the combination of four separate legitimate platforms — Discord, Slack, Outlook, and file.io — with Go malware suggests a deliberate architecture designed for long-term operational resilience. Taking down one platform doesn't kill the campaign.

---

# Incident #006 — May 5, 2026

**Target:** Organizations in India and Russia — industrial, consulting, retail, and transportation sectors

**Sector:** Industry / Consulting / Retail / Transportation

**Threat Actor:** Silver Fox — China-based cybercrime group, dual-track espionage and financial operations

**Origin:** China — assessed

**Source:** The Hacker News — May 4, 2026 | Kaspersky (Securelist) | CloudSEK | S2W

**Attack Type:** Tax-Themed Phishing → RustSL Loader → ValleyRAT → ABCDoor Python Backdoor

**Labels:** Silver Fox | ABCDoor | ValleyRAT | RustSL | Phantom Persistence | India | Russia | Tax Phishing | Rust Loader | Python Backdoor | Geofencing | China-Nexus

---

## Analysis

Silver Fox — a China-based cybercrime group operating a dual-track model of financial crime and espionage since 2024 — ran two consecutive tax-themed phishing waves against India and Russia between December 2025 and February 2026. Over 1,600 phishing emails were flagged across the campaign window. Both waves used nearly identical structure: emails impersonating the Income Tax Department of India or Russian tax authority equivalents, containing PDF files with two clickable links pointing to ZIP or RAR archives hosted on abc.haijing88[.]com.

The archive contains an executable masquerading as a PDF — actually a modified version of RustSL, an open-source Rust-based shellcode loader and AV bypass framework pulled from GitHub and customized by Silver Fox. The customized version adds country-based geofencing — targeting India, Indonesia, South Africa, Russia, and Cambodia — and sandbox/VM detection checks before unpacking the encrypted payload. One variant uses Phantom Persistence, a novel technique documented in June 2025 that abuses the Windows shutdown and reboot update sequence: the loader intercepts the system shutdown signal, halts normal shutdown, and forces execution at OS startup under the guise of a system update.

The encrypted payload is ValleyRAT (aka Winos 4.0), with its core component login-module.dll_bin handling C2, command execution, and module retrieval. One of the custom modules deployed after a second geofencing check is ABCDoor — a previously undocumented Python-based backdoor in Silver Fox's arsenal since at least December 19, 2024, deployed in attacks from February or March 2025 onward. ABCDoor contacts an external server via HTTPS and handles persistence, backdoor updates, screenshot capture, remote mouse and keyboard control, file system operations, process management, and clipboard exfiltration. A JavaScript loader variant of ABCDoor has also been observed, distributed via self-extracting SFX archives inside ZIP files.

The highest attack volumes were detected in India, Russia, and Indonesia, with South Africa and Japan also in scope. Newer RustSL variants have expanded targeting to Japan. S2W assessed Silver Fox as having evolved into a dual-track operation running extensive opportunistic financial campaigns alongside targeted espionage — using highly customized spearphishing tailored to the seasonal issues of each target country.

---

## Key Technical Indicators:
- **Threat actor:** Silver Fox — China-based, dual-track financial crime and espionage
- **Phishing volume:** 1,600+ emails, January–February 2026
- **Delivery:** PDF with clickable links → ZIP/RAR archive at abc.haijing88[.]com
- **Loader:** RustSL — modified Rust-based shellcode loader; custom geofencing (India, Indonesia, South Africa, Russia, Cambodia); VM/sandbox detection
- **Persistence technique:** Phantom Persistence — abuses Windows shutdown/reboot update sequence to execute at OS startup
- **Core payload:** ValleyRAT (Winos 4.0) — login-module.dll_bin handles C2, command execution, module retrieval
- **Final payload:** ABCDoor — Python-based backdoor; HTTPS C2; screenshot, keyboard/mouse control, file ops, clipboard exfil
- **ABCDoor JS variant:** delivered via SFX archives inside ZIP files
- *Second geofencing check before ABCDoor deployment*
- **Highest volume targets:** India, Russia, Indonesia; secondary: South Africa, Japan
- *Campaign active since December 2024; RustSL first Silver Fox use December 2025*

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.001 — Phishing: Spearphishing Attachment: tax-themed PDF delivered via phishing email impersonating Income Tax Department of India and Russian tax authority

- **TA0002 — Execution**
  - T1059.006 — Command and Scripting Interpreter: Python: ABCDoor Python backdoor executes commands on compromised host
  - T1059.007 — Command and Scripting Interpreter: JavaScript: JavaScript loader variant of ABCDoor executes via SFX archive

- **TA0003 — Persistence**
  - T1547.001 — Boot or Logon Autostart Execution: Registry Run Keys: Phantom Persistence abuses Windows shutdown/reboot sequence to force loader execution at OS startup

- **TA0005 — Defense Evasion**
  - T1027 — Obfuscated Files or Information: RustSL unpacks encrypted payload; ABCDoor delivered inside SFX archives inside ZIP archives
  - T1497 — Virtualization/Sandbox Evasion: RustSL performs VM and sandbox detection before payload execution
  - T1036 — Masquerading: archive executable mimics PDF file; loader masquerades as system update during Phantom Persistence execution
  - T1014 — Rootkit: geofencing check filters non-target systems before ABCDoor deployment

- **TA0006 — Credential Access**
  - T1115 — Clipboard Data: ABCDoor exfiltrates clipboard contents

- **TA0009 — Collection**
  - T1113 — Screen Capture: ABCDoor captures screenshots
  - T1005 — Data from Local System: file system operations conducted via ABCDoor
  - T1115 — Clipboard Data: clipboard contents collected and exfiltrated

- **TA0011 — Command & Control**
  - T1071.001 — Application Layer Protocol: Web Protocols: ABCDoor contacts external C2 server via HTTPS
  - T1105 — Ingress Tool Transfer: ValleyRAT retrieves and executes additional modules including ABCDoor post-compromise

- **TA0040 — Impact**
  - T1496 — Resource Hijacking: remote mouse and keyboard control via ABCDoor enables full interactive access to compromised systems

---

## Strategic Context

India is the primary target. The campaign impersonated the Income Tax Department of India specifically — a tax lure timed to India's filing season, tailored to provoke urgency in Indian corporate and industrial recipients. Silver Fox's model of adapting phishing themes to the seasonal calendar of each target country shows a level of operational planning that goes beyond opportunistic cybercrime.

The dual-track nature of Silver Fox matters here. Financial crime and espionage running simultaneously from the same infrastructure means victim organizations face both data theft and potential financial loss from the same campaign. The industrial and consulting sector targeting in India suggests Silver Fox is after business intelligence, supply chain data, and potentially defense-adjacent information given India's current security environment.

ABCDoor's full remote control capability — keyboard, mouse, screenshots, file operations — makes it a complete espionage platform, not just an infostealer. Combined with ValleyRAT's modular architecture, Silver Fox has a framework that can be repurposed for any collection objective depending on what the victim environment contains.

---

# Incident #007 — May 5, 2026

**Target:** Philippine government and military domains (*.mil.ph, *.ph); Laotian government (*.gov.la); MSPs and hosting providers in Philippines, Laos, Canada, South Africa, and the US

**Sector:** Government / Military / Managed Service Providers / Hosting Infrastructure

**Threat Actor:** Unknown — unattributed; single confirmed attacker IP 95.111.250[.]175

**Origin:** Unattributed

**Source:** The Hacker News — May 3, 2026 | Ctrl-Alt-Intel — May 2, 2026

**Attack Type:** cPanel CVE-2026-41940 Exploitation → Authentication Bypass → Government and Military Infrastructure Targeting

**Labels:** CVE-2026-41940 | cPanel | WHM | Authentication Bypass | Philippines | Laos | Government | Military | MSP | Active Exploitation | Southeast Asia

---

## Analysis

Ctrl-Alt-Intel detected active exploitation of CVE-2026-41940 — the critical cPanel/WHM authentication bypass — against government and military infrastructure on May 2, 2026, two days after the public PoC was released. The attacks originated from a single IP address — 95.111.250[.]175 — primarily targeting Philippine military domains (*.mil.ph), Philippine government domains (*.ph), and Laotian government domains (*.gov.la), with a secondary cluster targeting managed service providers and hosting providers in the Philippines, Laos, Canada, South Africa, and the US.

The targeting pattern is deliberately chosen. Philippine and Laotian government and military infrastructure running cPanel-based hosting is a specific and narrow target set — this is not mass opportunistic scanning. The actor went straight for government and military domains, then MSPs and hosting providers that likely serve additional government clients. Compromising an MSP or hosting provider that manages government accounts is a force multiplier — one entry point, many downstream victims.

CVE-2026-41940 was already being exploited as a zero-day since February 23, with CISA adding it to the KEV catalog and Sorry ransomware operators confirmed using it for mass attacks. This Southeast Asian government targeting represents a distinct campaign track running parallel to the ransomware exploitation — same CVE, different actor, different objective. The ransomware operators want money; this actor appears to want access.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-41940 — cPanel/WHM CVSS 9.8 authentication bypass
- **Attacker IP:** 95.111.250[.]175
- **Detection date:** May 2, 2026 — two days post-PoC release
- **Primary targets:** *.mil.ph (Philippine military), *.ph (Philippine government), *.gov.la (Laotian government)
- **Secondary targets:** MSPs and hosting providers — Philippines, Laos, Canada, South Africa, US
- **Attack method:** publicly available PoC used directly
- **Objective assessed:** government/military access, not ransomware

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: CVE-2026-41940 exploited against internet-facing cPanel/WHM instances serving government and military domains

- **TA0004 — Privilege Escalation**
  - T1068 — Exploitation for Privilege Escalation: authentication bypass yields root-level WHM access without valid credentials

- **TA0007 — Discovery**
  - T1595.002 — Active Scanning: Vulnerability Scanning: targeted scanning of government and military domains for cPanel/WHM exposure

- **TA0008 — Lateral Movement**
  - T1199 — Trusted Relationship: MSP and hosting provider targeting enables pivot into downstream government client infrastructure

---

## Strategic Context

Philippine and Laotian military domains being the primary targets is significant regional context. The Philippines has been at the center of ongoing South China Sea tensions with China, and Laos sits within China's direct sphere of influence while maintaining complex relationships with Western-aligned partners. An unknown actor targeting both simultaneously — along with their MSP infrastructure — suggests intelligence collection objectives aligned with regional power competition rather than ransomware or financial crime.

This entry connects directly to Global-Watch #002 — the original cPanel CVE-2026-41940 disclosure. What started as a mass-exploitation zero-day being used by Sorry ransomware operators has now also been picked up by a separate actor running targeted government and military campaigns against Southeast Asian infrastructure. The same CVE, within days of the PoC going public, is now serving two completely different threat actor objectives simultaneously. That's the exploitation lifecycle in 2026 — critical CVEs get absorbed into multiple actor toolsets within days of public disclosure.

---

# Incident #008 — May 6, 2026

**Target:** Enterprise organizations globally using Weaver E-cology OA platform — primarily Chinese enterprise deployments

**Sector:** Enterprise / Office Automation / Collaboration Platforms

**Threat Actor:** Unknown — unattributed; single campaign tracked by Vega Research Team

**Origin:** Unattributed

**Source:** The Hacker News — May 5, 2026 | Vega Research Team | Shadowserver | QiAnXin

**Attack Type:** Unauthenticated RCE via Exposed Debug API Endpoint — Active Exploitation Since March 17

**Labels:** CVE-2026-22679 | CVSS 9.8 | Weaver E-cology | Fanwei | OA Platform | Debug API | Unauthenticated RCE | Active Exploitation | MSI Implant | Enterprise

---

## Analysis

CVE-2026-22679 is a CVSS 9.8 unauthenticated RCE in Weaver E-cology 10.0 — a widely deployed Chinese enterprise office automation and collaboration platform. The vulnerable endpoint is /papi/esearch/data/devops/dubboApi/debug/method, a debug API left exposed in production. Attackers craft POST requests with attacker-controlled interfaceName and methodName parameters to reach command-execution helpers and run arbitrary commands on the system with no authentication required.

Weaver shipped patches on March 12, 2026. QiAnXin reproduced the RCE on March 17 — the same day Vega Research Team identified the earliest evidence of active exploitation. That's a five-day patch-to-exploit window, with Shadowserver observing first signs of exploitation on March 31. The exploiting actor ran a week of operator activity: initial RCE verification, three failed payload drops, an attempted pivot to an MSI implant named fanwei0324.msi — using the romanized Chinese name for Weaver to disguise the malicious installer — followed by attempts to pull PowerShell payloads from attacker-controlled infrastructure. **Standard discovery commands were run throughout:** whoami, ipconfig, tasklist.

A Python-based detection script is now publicly available, identifying vulnerable instances by checking if the susceptible API endpoint is accessible — lowering the bar for both defenders and attackers.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-22679, CVSS 9.8
- **Vulnerable endpoint:** /papi/esearch/data/devops/dubboApi/debug/method — exposed debug API in production
- **Attack method:** POST request with attacker-controlled interfaceName and methodName parameters → arbitrary command execution
- **Affected versions:** Weaver E-cology 10.0 prior to 20260312
- **Patch released:** March 12, 2026
- **First exploitation:** March 17, 2026 — five days post-patch
- **Shadowserver first observed exploitation:** March 31, 2026
- **MSI implant name:** fanwei0324.msi — masquerading as legitimate Weaver software
- **Discovery commands observed:** whoami, ipconfig, tasklist
- *PowerShell payload retrieval attempted from attacker-controlled infrastructure*
- **Detection script:** Python-based, publicly available — checks for accessible vulnerable endpoint

---

> **Entry Type:** Active exploitation confirmed — unattributed threat actor. No MITRE ATT&CK tags applied.

---

## Strategic Context

A debug endpoint left exposed in a production enterprise platform is a fundamental security hygiene failure. Weaver E-cology is used across Chinese enterprise and government environments — a platform that processes internal communications, workflows, and approvals. RCE on that surface isn't just a server compromise, it's access to internal business operations data.

The MSI naming convention — using the romanized company name plus a date — is a low-effort but effective social engineering layer. An IT admin seeing fanwei0324.msi in a process list might assume it's a legitimate Weaver update rather than a malicious implant. Small detail, deliberate choice.

The public detection script is a double-edged tool. Defenders can use it to identify exposure. So can attackers scanning for targets at scale.

---

# Incident #009 — May 6, 2026

**Target:** MetInfo CMS deployments globally — primarily China and Hong Kong

**Sector:** Web Infrastructure / Content Management

**Threat Actor:** Unknown — opportunistic, automated scanning confirmed; surge activity from China and Hong Kong IP addresses

**Origin:** Unattributed

**Source:** The Hacker News — May 5, 2026 | VulnCheck (Caitlin Condon) | Egidio Romano (KarmaInsecurity)

**Attack Type:** Unauthenticated PHP Code Injection → Remote Code Execution

**Labels:** CVE-2026-29014 | CVSS 9.8 | MetInfo CMS | PHP Code Injection | WeChat API | Unauthenticated RCE | Active Exploitation | China | Hong Kong | Honeypot

---

## Analysis

CVE-2026-29014 is a CVSS 9.8 unauthenticated PHP code injection flaw in MetInfo CMS versions 7.9, 8.0, and 8.1 — an open-source CMS with approximately 2,000 internet-exposed instances, most in China. The vulnerability is in /app/system/weixin/include/class/weixinreply.class.php, the script handling WeChat API requests. Insufficient sanitization of user-supplied input allows remote unauthenticated attackers to inject and execute arbitrary PHP code. On non-Windows servers, one prerequisite applies — the /cache/weixin/ directory must exist, which is created automatically when the official WeChat plugin is installed.

MetInfo patched on April 7, 2026. Exploitation began April 25 — an 18-day window — initially as sparse automated honeypot probing in the US and Singapore. On May 1 activity surged, with attack traffic focusing on China and Hong Kong IP addresses. The narrow geographic focus of the surge suggests either targeted scanning of Chinese-language CMS deployments or actors operating from within China's network space probing domestic targets.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-29014, CVSS 9.8
- **Vulnerable component:** /app/system/weixin/include/class/weixinreply.class.php — WeChat API request handler
- **Attack method:** unsanitized user input in WeChat API flow → arbitrary PHP code injection and execution
- **Prerequisite (non-Windows):** /cache/weixin/ directory must exist — created by WeChat plugin installation
- **Affected versions:** MetInfo CMS 7.9, 8.0, 8.1
- **Patch released:** April 7, 2026
- **First exploitation:** April 25, 2026 — 18 days post-patch
- **Surge activity:** May 1, 2026 — China and Hong Kong IP addresses
- **Exposed instances:** ~2,000 globally — majority in China
- **Initial activity:** sparse automated honeypot probing in US and Singapore

---

> **Entry Type:** Active exploitation confirmed — unattributed opportunistic actor. No MITRE ATT&CK tags applied.

---

## Strategic Context

2,000 exposed instances is a small attack surface by global standards but the geographic concentration matters. MetInfo is a Chinese-language CMS — most of its deployments are Chinese businesses and organizations. A surge in exploitation traffic from China and Hong Kong IP addresses against a Chinese CMS suggests domestic targeting, whether for defacement, data theft, or establishing footholds in Chinese business infrastructure.

The WeChat plugin dependency as a prerequisite is worth noting. It narrows the exploitable pool slightly but the WeChat plugin is standard for Chinese business websites — any MetInfo deployment operating in China's digital ecosystem almost certainly has it installed.

---

# Incident #010 — May 7, 2026

**Target:** Apache HTTP Server deployments globally — any server running mod_http2 with multi-threaded MPM; Debian-derived systems and official Docker images at higher RCE risk

**Sector:** Web Infrastructure / Server Security

**Threat Actor:** N/A — Vulnerability Disclosure

**Origin:** Discovered by Bartlomiej Dmitruk (Striga.ai) and Stanislaw Strzalkowski (ISEC.pl)

**Source:** The Hacker News — May 5, 2026 | Apache Software Foundation Advisory

**Vulnerability Class:** Double-Free in HTTP/2 Stream Cleanup → DoS and Potential RCE — No Exploitation Confirmed

**Labels:** CVE-2026-23918 | CVSS 8.8 | Apache HTTP Server | HTTP/2 | Double-Free | DoS | RCE | mod_http2 | Debian | Docker | Working PoC

---

## Analysis

CVE-2026-23918 is a double-free vulnerability in Apache HTTP Server 2.4.66's mod_http2 module, specifically in the stream cleanup path of h2_mplx.c. The trigger is precise: a client sends an HTTP/2 HEADERS frame immediately followed by RST_STREAM with a non-zero error code on the same stream, before the multiplexer has registered the stream. Two nghttp2 callbacks fire in sequence — on_frame_recv_cb for the RST and on_stream_close_cb for the close — and both call h2_mplx_c1_client_rst → m_stream_cleanup, pushing the same h2_stream pointer onto the spurge cleanup array twice. When c1_purge_streams iterates and calls h2_stream_destroy → apr_pool_destroy on each entry, the second call hits already-freed memory.

The DoS outcome is trivial — one TCP connection, two frames, no authentication, no special headers, no specific URL, and the worker crashes. Apache respawns the worker but every request on the crashed worker is dropped, and the pattern can be sustained indefinitely. MPM prefork is not affected; any multi-threaded MPM is.

The RCE path is more constrained but demonstrated in lab conditions. The chain places a fake h2_stream struct at the freed virtual address via mmap reuse, points its pool cleanup function to system(), and uses Apache's scoreboard memory as a stable container for the fake structures and the command string. The scoreboard sits at a fixed address for the lifetime of the server even with ASLR — that fixed address is what makes the RCE path practical. The RCE path requires an Apache Portable Runtime with the mmap allocator, which is the default on Debian-derived systems and the official Apache Docker image. Practical exploitation also requires an info leak for system() and scoreboard offsets, and the heap spray is probabilistic — but researchers achieved execution in minutes in lab conditions.

Fixed in Apache HTTP Server 2.4.67. No exploitation confirmed in the wild.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-23918, CVSS 8.8
- **Affected version:** Apache HTTP Server 2.4.66 with mod_http2
- **Root cause:** double-free in h2_mplx.c stream cleanup path — same h2_stream pointer pushed to spurge array twice via two sequential nghttp2 callbacks
- **Trigger:** HEADERS frame followed immediately by RST_STREAM with non-zero error code before multiplexer stream registration
- **DoS:** trivial — one TCP connection, two frames, no authentication required; sustained crash pattern possible
- **RCE path:** requires APR mmap allocator (default on Debian and official Docker image); scoreboard fixed address bypasses ASLR; heap spray probabilistic; execution in minutes in lab
- **MPM prefork:** not affected
- **Fixed:** Apache HTTP Server 2.4.67
- *No confirmed exploitation in the wild*

---

> **Entry Type:** Vulnerability Disclosure — Striga.ai / ISEC.pl research. DoS trivial, RCE demonstrated in lab. No confirmed exploitation. MITRE ATT&CK tags not applicable.

---

## Strategic Context

Apache HTTP Server is one of the most widely deployed web servers in the world. mod_http2 ships in default builds and HTTP/2 is widely enabled in production. The DoS path requiring no authentication and just two frames means any internet-facing Apache 2.4.66 deployment is a trivial crash target. The RCE path is more complex but the scoreboard ASLR bypass is the key detail — it's a stable primitive that survives server restarts, which makes it a reliable building block for a full exploit chain once an info leak is available.

Debian-derived systems and Docker deployments are at elevated risk for RCE specifically due to the mmap allocator default. Organizations running Apache in containers should treat this as urgent — Docker images are often left unpatched longer than bare-metal deployments.

---

# Incident #011 — May 7, 2026

**Target:** Organizations globally running Palo Alto Networks PA-Series and VM-Series firewalls with User-ID Authentication Portal exposed to internet or untrusted networks

**Sector:** Network Security / Firewall Infrastructure

**Threat Actor:** Unknown — limited exploitation confirmed

**Origin:** Unattributed

**Source:** SecurityWeek — May 6, 2026 | Palo Alto Networks Advisory

**Vulnerability Class:** Buffer Overflow in PAN-OS Captive Portal → Unauthenticated RCE as Root — Limited Active Exploitation

**Labels:** CVE-2026-0300 | CVSS 9.3 | PAN-OS | Palo Alto | Captive Portal | Buffer Overflow | Unauthenticated RCE | Active Exploitation | Firewall | PA-Series | VM-Series

---

## Analysis

CVE-2026-0300 is a buffer overflow in the User-ID Authentication Portal (Captive Portal) service of Palo Alto Networks PAN-OS software, affecting PA-Series and VM-Series firewalls. Specially crafted packets sent to the Captive Portal service allow an unauthenticated attacker to execute arbitrary code with root privileges. CVSS scores at 9.3 when the portal is accessible from the internet or untrusted networks, dropping to 8.7 when access is restricted to trusted internal IPs only.

Palo Alto Networks confirmed limited exploitation in the wild in its advisory. No threat actor has been identified. The vulnerability sits on the network perimeter — the same layer as the Cisco ASA FIRESTARTER campaign documented in Global-Watch #039. Firewall and VPN appliances as an attack surface have been a consistent APT priority throughout 2025 and into 2026, with state-sponsored actors specifically targeting perimeter devices for persistent access and traffic interception.

## Key Technical Indicators:
- **CVE:** CVE-2026-0300, CVSS 9.3 (internet-exposed) / 8.7 (restricted access)
- **Vulnerable component:** User-ID Authentication Portal (Captive Portal) service — PAN-OS on PA-Series and VM-Series firewalls
- **Attack method:** specially crafted packets to Captive Portal → buffer overflow → unauthenticated RCE as root
- **Active exploitation:** confirmed — described as "limited" by Palo Alto Networks
- *No threat actor identified at time of disclosure*
- **Mitigation:** restrict Captive Portal access to trusted internal IPs only if immediate patching is not possible

---

> **Entry Type:** Active exploitation confirmed — unattributed. MITRE ATT&CK tags not applicable.

---

## Strategic Context

Network perimeter devices getting hit with unauthenticated RCE is a pattern this log has documented repeatedly — Cisco ASA FIRESTARTER (#039), cPanel (#002, #007), and now PAN-OS. Firewalls and VPN appliances are high-value targets because they sit at the boundary between external and internal networks. Root access on a Palo Alto firewall means full visibility into all traffic passing through it and a natural pivot point into the internal network behind it.

The "limited exploitation" qualifier from Palo Alto is worth reading carefully. Vendors typically use that language when they have confirmed evidence of exploitation but don't yet have full visibility into campaign scope. It doesn't mean low-risk — it means early stage.

## Update — May 8, 2026:

BleepingComputer and CISA confirmed exploitation of CVE-2026-0300 began as early as April 9, 2026 — 28 days before public disclosure, making this a confirmed zero-day window. CISA added it to the KEV catalog. Official patches are expected May 13, 2026. Until then Palo Alto recommends restricting Captive Portal access to trusted zones only and disabling Response Pages in the Interface Management Profile for L3 interfaces where untrusted traffic can ingress.

---

# Incident #012 — May 7, 2026

**Target:** DAEMON Tools users globally — Windows disk image mounting software; backdoor selectively deployed to approximately a dozen systems

**Sector:** Software Supply Chain / Consumer and Enterprise Software

**Threat Actor:** Unknown — Chinese-speaking artifacts identified in malicious implants

**Origin:** China-speaking — assessed from artifact analysis; not confirmed state-sponsored

**Source:** SecurityWeek — May 6, 2026 | Securelist (Kaspersky) | AVB Disc Soft notification

**Attack Type:** Trojanized Software Installer — Supply Chain Compromise / Selective Backdoor Deployment

**Labels:** DAEMON Tools | Supply Chain | Trojanized Installer | Chinese-Speaking | Signed Certificate | Selective Deployment | AVB Disc Soft | Backdoor | Windows

---

## Analysis

Kaspersky researchers identified that DAEMON Tools installers distributed from the official AVB Disc Soft website were trojanized with a malicious payload starting April 8, 2026 — a supply chain compromise that remained active at time of reporting. Versions 12.5.0.2421 through 12.5.0.2434 are confirmed affected. The installers are signed with legitimate digital certificates belonging to DAEMON Tools developers, meaning standard certificate-based trust checks pass without flagging.

Despite worldwide distribution of the trojanized installers, the full backdoor payload was dropped on only approximately a dozen systems. The selective deployment is the most operationally significant detail — it indicates the actor is not conducting mass exploitation but rather triaging downloads for specific targets of interest before deploying the implant. That triage process suggests either automated victim profiling or manual review of incoming infections before committing to the full backdoor.

Chinese-speaking artifacts were identified in the malicious implants. AVB Disc Soft was notified by Kaspersky to take remediation steps. No confirmed attribution beyond the language artifacts.

---

## Key Technical Indicators:
- **Compromised software:** DAEMON Tools versions 12.5.0.2421 – 12.5.0.2434
- **Distribution:** official AVB Disc Soft website — legitimate installer delivery channel
- **Signed:** valid DAEMON Tools developer digital certificates — passes certificate trust checks
- **Active since:** April 8, 2026 — ongoing at time of reporting
- **Backdoor deployment:** worldwide trojanized installer distribution; full backdoor dropped on ~12 systems only — selective targeting confirmed
- **Attribution artifacts:** Chinese-speaking strings identified in malicious implants
- *AVB Disc Soft notified by Kaspersky for remediation*

---

> **Entry Type:** Active supply chain compromise — unattributed beyond Chinese-speaking artifact assessment. MITRE ATT&CK tags not applicable — no confirmed threat actor.

---

## Strategic Context

Signed installers from a legitimate vendor website is a supply chain attack that bypasses the most common user-facing security advice — only download from official sources, verify the signature. Both checks pass here. That's why software supply chain attacks are particularly dangerous: the trust model that protects against most malware is the delivery mechanism.

The selective backdoor deployment is the pattern worth watching. Mass trojanization with targeted payload delivery is an increasingly common operational pattern — cast a wide net through a trusted software channel, then surgically deploy to victims of actual intelligence value. It's resource-efficient and keeps the implant footprint small, which extends operational life before detection.

The Chinese-speaking artifact is insufficient for firm attribution but consistent with the pattern of China-aligned actors using supply chain vectors seen in SHADOW-EARTH-053 (#004) and GopherWhisper (#005) this month.

---

# Incident #013 — May 8, 2026

**Target:** Enterprise organizations globally running on-premises Ivanti Endpoint Manager Mobile (EPMM)

**Sector:** Enterprise / Mobile Device Management / Endpoint Security

**Threat Actor:** Unknown — very limited exploitation confirmed at time of disclosure

**Origin:** Unattributed

**Source:** The Hacker News — May 7, 2026 | BleepingComputer — May 7, 2026 | Ivanti Security Advisory

**Vulnerability Class:** Improper Input Validation → Authenticated RCE — Zero-Day Active Exploitation

**Labels:** CVE-2026-6973 | CVSS 7.2 | Ivanti EPMM | MDM | RCE | Zero-Day | Active Exploitation | Enterprise | MobileIron | CISA KEV

---

## Analysis

Ivanti disclosed CVE-2026-6973 on May 7, 2026 — a high-severity improper input validation flaw in Endpoint Manager Mobile (EPMM) versions 12.8.0.0 and earlier that allows a remotely authenticated attacker with administrative access to execute arbitrary code. Exploitation was confirmed at time of disclosure, with Ivanti describing it as affecting a very limited number of customers. No threat actor has been identified and the end goals of confirmed attacks are unknown.

The authentication requirement is the key differentiator from prior Ivanti EPMM zero-days. CVE-2026-1281 and CVE-2026-1340 — disclosed in January 2026 and exploited in zero-day attacks — were unauthenticated. CVE-2026-6973 requires admin credentials, which is why Ivanti's mitigation advice specifically ties back to the January incidents: customers who rotated credentials after the January compromise have significantly reduced risk. That link between January and May exploitation suggests the actor behind CVE-2026-6973 may be reusing admin credentials harvested in the January campaign — a credential reuse chain across two separate zero-day windows.

Ivanti simultaneously patched four additional high-severity EPMM flaws — CVE-2026-5786 (improper access control, admin access), CVE-2026-5787 (improper certificate validation, Sentry host impersonation and CA-signed cert theft), CVE-2026-5788 (improper access control, unauthenticated arbitrary method invocation), and CVE-2026-7821 (improper certificate validation, restricted device enrollment bypass, information disclosure). None of the four additional CVEs have confirmed exploitation. CVE-2026-5787 and CVE-2026-7821 are particularly notable — unauthenticated certificate impersonation and device enrollment bypass are high-value capabilities for any actor targeting MDM infrastructure.

Shadowserver tracks over 850 internet-exposed EPMM IP addresses — 508 in Europe, 182 in North America. CISA has now listed 33 Ivanti vulnerabilities as exploited in the wild, with 12 linked to ransomware operations.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-6973, CVSS 7.2 — improper input validation, authenticated RCE with admin privileges
- **Affected versions:** EPMM 12.8.0.0 and earlier
- **Fixed versions:** 12.6.1.1, 12.7.0.1, 12.8.0.1
- *Exploitation confirmed at disclosure — very limited number of customers*
- *Credential reuse suspected from January 2026 CVE-2026-1281/CVE-2026-1340 compromise*
- **Additional CVEs patched:** CVE-2026-5786 (admin access), CVE-2026-5787 (Sentry impersonation/cert theft), CVE-2026-5788 (unauthenticated method invocation), CVE-2026-7821 (device enrollment bypass)
- **Exposed instances:** 850+ per Shadowserver — 508 Europe, 182 North America
- **CISA KEV:** 33 Ivanti CVEs listed exploited; 12 linked to ransomware
- **Scope:** on-premises EPMM only — cloud Neurons for MDM not affected

---

> **Entry Type:** Active exploitation confirmed — unattributed. MITRE ATT&CK tags not applicable.

---

## Strategic Context

This is the third Ivanti EPMM zero-day exploitation window in 2026 — January's unauthenticated pair, April's CISA KEV order, and now May's authenticated RCE. The pattern is consistent enough that Ivanti EPMM should be treated as a persistently targeted platform, not an occasional victim. Previous zero-day campaigns against EPMM have been attributed to Chinese state-sponsored groups, though no attribution has been confirmed for the current exploitation.

The credential reuse angle is the most operationally significant detail. If the actor behind CVE-2026-6973 is operating on admin credentials harvested in January, then patching CVE-2026-6973 alone doesn't close the access. Organizations that were compromised in January and didn't rotate credentials are potentially facing a persistent threat actor who has been inside their MDM infrastructure for months.

MDM platforms are high-value targets because they manage every mobile device in an organization — certificates, email profiles, VPN configurations, app deployment. Root access to EPMM is effectively administrative access to the entire mobile fleet.

---

# Incident #014 — May 8, 2026

**Target:** Applications and platforms using vm2 Node.js sandbox library — any environment running untrusted JavaScript in vm2

**Sector:** Developer Infrastructure / JavaScript Runtime Security

**Threat Actor:** N/A — Vulnerability Disclosure

**Origin:** Discovered by multiple researchers

**Source:** The Hacker News — May 7, 2026

**Vulnerability Class:** Sandbox Escape via JavaScript Prototype Abuse → Arbitrary Code Execution on Host — No Exploitation Confirmed

**Labels:** CVE-2026-24118 | CVE-2026-24120 | CVSS 9.8 | vm2 | Node.js | Sandbox Escape | JavaScript | RCE | Developer Infrastructure

---

## Analysis

Twelve critical security vulnerabilities were disclosed in vm2 — an open-source Node.js library used to run untrusted JavaScript code inside a secure sandbox by intercepting and proxying JavaScript objects to prevent sandboxed code from accessing the host environment. The two most critical are CVE-2026-24118 and CVE-2026-24120, both CVSS 9.8.

CVE-2026-24118 allows sandbox escape via the __lookupGetter__ method, enabling arbitrary code execution on the underlying host. CVE-2026-24120 is a patch bypass for CVE-2023-37466 — a prior critical sandbox escape — exploiting the species property of promise objects to escape the sandbox and execute arbitrary commands. The fact that CVE-2026-24120 directly bypasses a prior patch signals that the sandbox boundary in vm2 has a structural weakness that point fixes are not fully addressing.

vm2 is used anywhere developers need to safely run untrusted or user-supplied JavaScript — serverless platforms, online code execution environments, plugin systems, CI/CD sandboxes. A sandbox escape in vm2 means code running inside what should be an isolated environment can break out and execute commands on the host system. Fixes are available in vm2 version 3.11.0 for CVE-2026-24118 (affects versions ≤ 3.10.4) and a corresponding patch for CVE-2026-24120.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-24118, CVSS 9.8 — sandbox escape via __lookupGetter__; affects vm2 ≤ 3.10.4; fixed in 3.11.0
- **CVE:** CVE-2026-24120, CVSS 9.8 — patch bypass for CVE-2023-37466 via promise species property; sandbox escape to arbitrary command execution
- *12 total vulnerabilities disclosed across vm2*
- **Vulnerability class:** JavaScript prototype abuse enabling sandbox boundary bypass
- **Affected:** any application using vm2 to run untrusted JavaScript
- No exploitation confirmed in the wild

---

> **Entry Type:** Vulnerability Disclosure. No confirmed threat actor, no confirmed exploitation. MITRE ATT&CK tags not applicable.

---

## Strategic Context

vm2 being the sandbox library for untrusted JavaScript execution means this isn't just a developer tool bug — it's a foundational trust boundary failure. Applications that use vm2 to safely isolate user-supplied or third-party code are depending on it to hold that boundary. If vm2 can be escaped, the entire security model of those applications collapses.

CVE-2026-24120 being a bypass of a prior patch for CVE-2023-37466 is the detail worth watching. Three years after the original bypass, the same structural weakness in vm2's promise handling is still exploitable via a different property. That's not a patching failure — it's an architectural problem with how the sandbox handles JavaScript prototype chains. Organizations relying on vm2 for security-critical isolation should evaluate whether the library can provide the trust boundary they need or whether a more fundamental architectural change is required.

---

# Incident #015 — May 9, 2026

**Target:** Organizations globally running Dell RecoverPoint for Virtual Machines — VMware virtual infrastructure operators

**Sector:** Enterprise / Virtualization / Data Protection Infrastructure

**Threat Actor:** UNC6201 — suspected PRC-nexus threat cluster; overlaps with UNC5221 / Silk Typhoon (not confirmed same cluster)

**Origin:** China — assessed, PRC-nexus

**Source:** Mandiant / Google Threat Intelligence Group — February 17, 2026

**Attack Type:** Hard-Coded Default Credential Exploitation → WAR File Deployment → SLAYSTYLE/BRICKSTORM/GRIMBOLT Backdoor Chain → VMware Ghost NIC Pivoting

**Labels:** CVE-2026-22769 | CVSS 10.0 | UNC6201 | Dell RecoverPoint | GRIMBOLT | BRICKSTORM | SLAYSTYLE | VMware | Ghost NIC | SPA | iptables | China-Nexus | Espionage | Edge Appliance

---

## Analysis

Mandiant and Google Threat Intelligence Group disclosed zero-day exploitation of CVE-2026-22769 — a CVSS 10.0 flaw in Dell RecoverPoint for Virtual Machines — by UNC6201, a suspected PRC-nexus threat cluster with notable overlaps with UNC5221 (Silk Typhoon), though GTIG does not currently consider the two the same cluster. Active exploitation dates to at least mid-2024 - over 18 months of undetected access before disclosure.

The root cause is hard-coded default credentials for the admin user stored in plaintext in /home/kos/tomcat9/tomcat-users.xml on the Dell RecoverPoint appliance. Any actor who discovers these credentials can authenticate to the Apache Tomcat Manager, upload a malicious WAR file via the /manager/text/deploy endpoint, and execute arbitrary commands as root. No vulnerability exploitation in the traditional sense — the appliance ships with a known password and a management interface that accepts file uploads from anyone who has it.

UNC6201 used this access to deploy SLAYSTYLE — a Java-based web shell embedded in the malicious WAR file — followed by BRICKSTORM, a previously documented backdoor used in UNC6201 espionage campaigns. In September 2025 the group replaced BRICKSTORM binaries with GRIMBOLT — a new C# backdoor compiled using native ahead-of-time (AOT) compilation and packed with UPX. Native AOT removes the common intermediate language metadata typically present in .NET binaries, complicating static analysis and stripping the artifacts that most C# malware detection signatures rely on. GRIMBOLT provides a remote shell and uses the same C2 infrastructure as BRICKSTORM, suggesting the replacement was either a planned lifecycle rotation or a reaction to Mandiant's prior BRICKSTORM detection work.

Both BRICKSTORM and GRIMBOLT persist via modification of /home/kos/kbox/src/installation/distribution/convert_hosts.sh - a legitimate shell script executed at boot time via rc.local. The backdoor path is injected into this script, ensuring execution survives reboots.

The VMware activity is the most novel finding. UNC6201 created Ghost NICs — temporary virtual network ports on existing VMs running on ESXi — to pivot stealthily to internal and SaaS infrastructure without generating the network anomalies a new VM would create. Separately, iptables Single Packet Authorization was used on **compromised vCenter appliances:** incoming traffic on port 443 is monitored for a specific hex string, the source IP is whitelisted, and for the next 300 seconds any traffic from that IP to port 443 is silently redirected to port 10443 where the C2 listener operates. This SPA mechanism makes the backdoor invisible to port scanners and passive network monitors — the port appears closed until the correct knock sequence arrives.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-22769, CVSS 10.0 — hard-coded default admin credentials in /home/kos/tomcat9/tomcat-users.xml
- **Attack vector:** Tomcat Manager /manager/text/deploy endpoint — malicious WAR file upload as root
- **Web shell:** SLAYSTYLE — Java-based, deployed via WAR file
- **Backdoor chain:** BRICKSTORM → replaced by GRIMBOLT (September 2025)
- **GRIMBOLT:** C# backdoor, Native AOT compiled, UPX packed — complicates static analysis; remote shell capability
- **GRIMBOLT C2:** wss://149.248.11.71/rest/apisession (WebSocket)
- **Persistence:** convert_hosts.sh modification — executed at boot via rc.local
- **Ghost NICs:** temporary virtual network ports created on existing ESXi VMs for stealthy lateral movement
- **iptables SPA:** port 443 monitors for hex string → whitelists source IP → redirects to port 10443 for 300 seconds
- **Exploitation active since:** mid-2024 — 18+ months before disclosure
- **Attribution:** UNC6201 — PRC-nexus; overlaps with UNC5221/Silk Typhoon, not confirmed same cluster
- *YARA rules and IOCs published by GTIG — available in GTI Collection*
- **File hashes:** GRIMBOLT (24a11a26...), SLAYSTYLE (92fb4ad6...), BRICKSTORM (aa688682... and others)

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1078.001 — Valid Accounts: Default Accounts: hard-coded default admin credentials in tomcat-users.xml used to authenticate to Tomcat Manager

- **TA0002 — Execution**
  - T1505.003 — Server Software Component: Web Shell: SLAYSTYLE web shell deployed via malicious WAR file upload to Tomcat Manager
  - T1059.004 — Command and Scripting Interpreter: Unix Shell: arbitrary commands executed as root via SLAYSTYLE web shell

- **TA0003 — Persistence**
  - T1037 — Boot or Logon Initialization Scripts: BRICKSTORM and GRIMBOLT paths injected into convert_hosts.sh — executed at boot via rc.local
  - T1505.003 — Server Software Component: Web Shell: SLAYSTYLE maintains persistent web shell access on appliance

- **TA0005 — Defense Evasion**
  - T1027.002 — Obfuscated Files or Information: Software Packing: GRIMBOLT packed with UPX; Native AOT compilation removes CIL metadata to complicate static analysis
  - T1205 — Traffic Signaling: iptables Single Packet Authorization used to keep C2 port invisible — only responds after correct hex knock sequence
  - T1564.006 — Hide Artifacts: Run Virtual Instance: Ghost NICs created as temporary virtual network ports on existing VMs to avoid detection from new VM creation

- **TA0006 — Credential Access**
  - T1552.001 — Unsecured Credentials: Credentials in Files: hard-coded default credentials discovered in plaintext tomcat-users.xml configuration file

- **TA0008 — Lateral Movement**
  - T1021.001 — Remote Services: Remote Desktop Protocol: Ghost NICs used to pivot to internal and SaaS infrastructure via VMware virtual network layer
  - T1090.002 — Proxy: External Proxy: iptables redirects traffic through port 10443 after SPA knock — C2 traffic routed through appliance

- **TA0011 — Command & Control**
  - T1071.001 — Application Layer Protocol: Web Protocols: GRIMBOLT C2 via WebSocket (wss://) to 149.248.11.71
  - T1205 — Traffic Signaling: SPA iptables mechanism gates C2 access — port appears closed to scanners until correct packet received

- **TA0009 — Collection**
  - T1005 — Data from Local System: data collected from compromised Dell RecoverPoint and VMware infrastructure via SLAYSTYLE and GRIMBOLT

---

## Strategic Context

A CVSS 10.0 flaw caused by hard-coded default credentials is not a sophisticated zero-day — it's a fundamental product security failure. Dell shipped an appliance with a known admin password and an accessible management interface that accepts arbitrary code uploads. UNC6201 didn't need to find a memory corruption bug or chain multiple CVEs. They needed to know one password that was the same on every installation.

The Ghost NIC technique is the most operationally significant finding here. Creating temporary virtual NICs on existing VMs to pivot through VMware infrastructure is a novel evasion approach that leaves minimal artifacts — no new VM creation event, no new MAC address registration at the network layer, no anomalous process on the host. Defenders looking for lateral movement via new VM creation would miss this entirely. Expect this technique to appear in future VMware-focused intrusion reports as other groups adopt it.

The BRICKSTORM to GRIMBOLT replacement in September 2025 — after Mandiant's prior BRICKSTORM disclosure — shows UNC6201 actively monitoring defensive research and rotating tooling in response. **Native AOT compilation as an evasion choice is deliberate:** it directly addresses the detection signatures that C# malware typically generates. This is an actor that reads threat intelligence reports and adapts.

18 months of undetected access on Dell RecoverPoint appliances at multiple victim organizations is the operational headline. Edge appliances — VPN concentrators, backup infrastructure, storage controllers — continue to be the preferred entry and persistence layer for China-nexus APTs. They sit outside the EDR coverage of most enterprise environments, run embedded Linux with limited logging, and are rarely reimaged even after incidents.

---

# Incident #016 — May 12, 2026

**Target:** Government entities in South America — active since late 2024; government agencies in southeastern Europe — 2025

**Sector:** Government / Diplomatic

**Threat Actor:** UAT-8302 — China-nexus APT cluster; tooling shared with CL-STA-0049, Earth Alux, Earth Estries, Ink Dragon, Jewelbug, UNC5174, UNC6586, UAT-6382

**Origin:** China — assessed, China-nexus

**Source:** The Hacker News — May 5, 2026 | Cisco Talos (Jungsoo An, Asheer Malhotra, Brandon White)

**Attack Type:** Web Application Exploitation → Network Reconnaissance → Multi-Backdoor Deployment

**Labels:** UAT-8302 | NetDraft | NosyDoor | CloudSorcerer | SNOWRUST | VShell | Deed RAT | Draculoader | Stowaway | SoftEther | China-Nexus | South America | Southeast Europe | Shared Tooling | Premier Pass-as-a-Service

---

## Analysis

Cisco Talos disclosed UAT-8302 — a China-nexus APT cluster active since at least late 2024, targeting government entities in South America and southeastern Europe. The group's most defining characteristic is its toolset: nearly every malware family UAT-8302 deploys has been previously attributed to other China-aligned groups, indicating either shared infrastructure, a common upstream supplier, or a deliberate tool-sharing arrangement between clusters.

Initial access is suspected to involve zero-day and N-day exploitation of web applications — consistent with the broader China-nexus APT doctrine of targeting internet-facing services. Once inside, the group conducts extensive reconnaissance using open-source tools including gogo for automated network scanning, then moves laterally before deploying its final payloads.

The primary backdoor is NetDraft (aka NosyDoor) — a .NET-based backdoor that is a C# variant of FINALDRAFT/Squidoor, previously linked to Ink Dragon, CL-STA-0049, Earth Alux, Jewelbug, and REF7707. ESET tracks NosyDoor deployment to a group it calls LongNosedGoblin. The same malware has also been deployed against Russian IT organizations by Erudite Mogwai (Space Pirates/Webworm) under the name LuckyStrike Agent — meaning this single backdoor family has been used by at least five distinct China-aligned clusters and at least one Russia-adjacent group.

**Additional tools in UAT-8302's arsenal:** CloudSorcerer version 3.0 — a backdoor previously observed against Russian entities since May 2024; SNOWRUST — a Rust-based variant of SNOWLIGHT used by UNC5174, UNC6586, and UAT-6382 to download VShell; Deed RAT (Snappybee) — a ShadowPad successor used by Earth Estries; Zingdoor — also Earth Estries; and Draculoader — a shellcode loader delivering Crowdoor and HemiGate. For persistent alternative access, the group uses Stowaway and SoftEther VPN as proxy and tunneling tools.

The Premier Pass-as-a-Service model — disclosed by Trend Micro in October 2025 — provides context for the shared tooling. Earth Estries sells initial access to Earth Naga for follow-on exploitation, with the partnership assessed to have existed since at least late 2023. UAT-8302's broad tool overlap with multiple clusters is consistent with operating within or adjacent to this kind of access-sharing ecosystem.

---

## Key Technical Indicators:
- **Threat cluster:** UAT-8302 — active since late 2024; South America and southeastern Europe government targeting
- **Primary backdoor:** NetDraft/NosyDoor — .NET C# variant of FINALDRAFT/Squidoor; shared with Ink Dragon, CL-STA-0049, Earth Alux, Jewelbug, REF7707, LongNosedGoblin, Erudite Mogwai
- CloudSorcerer v3.0 — previously used against Russian entities since May 2024
- SNOWRUST — Rust-based SNOWLIGHT variant; downloads VShell payload from remote server
- Deed RAT (Snappybee) — ShadowPad successor; Earth Estries tooling
- Zingdoor — Earth Estries tooling
- Draculoader — shellcode loader delivering Crowdoor and HemiGate
- **Tunneling/proxy:** Stowaway, SoftEther VPN
- **Recon:** gogo open-source network scanner
- **Initial access:** suspected web application exploitation — zero-day and N-day
- **Premier Pass-as-a-Service context:** Earth Estries → Earth Naga access-sharing partnership since late 2023

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: zero-day and N-day exploitation of web applications suspected as primary initial access method

- **TA0002 — Execution**
  - T1059.001 — Command and Scripting Interpreter: PowerShell: post-exploitation commands executed via deployed backdoors
  - T1072 — Software Deployment Tools: VShell executed via SNOWRUST stager download

- **TA0003 — Persistence**
  - T1505 — Server Software Component: NetDraft/NosyDoor maintains persistent backdoor access on compromised government systems
  - T1133 — External Remote Services: SoftEther VPN and Stowaway used as alternative persistent access channels

- **TA0005 — Defense Evasion**
  - T1090 — Proxy: Stowaway and SoftEther VPN used to tunnel C2 traffic through legitimate-looking connections
  - T1036 — Masquerading: shared tooling across multiple clusters complicates attribution and analysis

- **TA0007 — Discovery**
  - T1046 — Network Service Discovery: gogo open-source scanner used for automated network reconnaissance post-compromise
  - T1083 — File and Directory Discovery: extensive network mapping conducted before final payload deployment

- **TA0008 — Lateral Movement**
  - T1021 — Remote Services: lateral movement conducted across government network environments before backdoor staging

- **TA0009 — Collection**
  - T1005 — Data from Local System: government data collected via NetDraft/NosyDoor and CloudSorcerer backdoors

- **TA0011 — Command & Control**
  - T1071 — Application Layer Protocol: NetDraft/NosyDoor C2 via application layer protocols
  - T1090 — Proxy: Stowaway proxy and SoftEther VPN tunnel C2 communications

---

## Strategic Context

UAT-8302's toolset reads like a catalogue of China-nexus APT malware from the past two years. NetDraft alone connects this cluster to at least five other documented groups. That level of overlap doesn't happen by accident — it reflects either a shared development infrastructure, a tool-lending arrangement, or participation in something like the Premier Pass-as-a-Service model where access and capabilities flow between clusters.

This connects directly to SHADOW-EARTH-053 documented in Global-Watch #004. Both are China-nexus APT clusters active in 2024-2026 using overlapping tooling — SHADOW-EARTH-053 uses ShadowPad and Godzilla while UAT-8302 uses Deed RAT (ShadowPad's successor) and NosyDoor which is shared with Earth Alux and CL-STA-0049 — both clusters documented in #003. The China APT ecosystem is operating as a networked infrastructure, not isolated actors.

South America as a targeting geography for China-nexus APTs is worth flagging. Government entities in South America are not a traditional China APT priority — the expansion into that region alongside the established southeastern Europe targeting suggests either specific intelligence requirements driving new geographic priorities or the maturation of the Premier Pass-as-a-Service model enabling broader targeting.

---

# Incident #017 — May 13, 2026

**Target:** Software developers globally — npm and PyPI ecosystem users; TanStack, Mistral AI, Guardrails AI, OpenSearch, UiPath, and 170+ package maintainers and their downstream users

**Sector:** Software Supply Chain / Developer Infrastructure / AI Tooling

**Threat Actor:** TeamPCP — Mini Shai-Hulud campaign, latest wave

**Origin:** Unknown — Russian locale skip present; geofenced destructive branch targeting Israel/Iran

**Source:** The Hacker News — May 12, 2026 | Aikido Security | Endor Labs |

**Attack Type:** GitHub Actions OIDC Token Hijacking → npm Worm Propagation → Credential Theft + Dead-Man's Switch Wiper

**Labels:** TeamPCP | Mini Shai-Hulud | TanStack | Mistral AI | Guardrails AI | OpenSearch | UiPath | npm | PyPI | OIDC | SLSA Bypass | GitHub Actions | Worm | Dead-Man's Switch | Wiper | Claude Code | VS Code | CI/CD | Supply Chain | CVE-2026-45321

---

## Analysis

TeamPCP's Mini Shai-Hulud campaign has escalated significantly. The latest wave compromised packages from TanStack, Mistral AI, Guardrails AI, OpenSearch, UiPath, and dozens of other maintainers across npm and PyPI — over 170 packages and 518 million cumulative downloads affected. 400 attacker-controlled repositories have been created containing the string **"Shai-Hulud: Here We Go Again."**

The TanStack compromise is the most technically significant entry point. Rather than stealing an npm token directly, TeamPCP exploited a chain of three GitHub Actions weaknesses: the pull_request_target trigger, GitHub Actions cache poisoning, and runtime memory extraction of an OIDC token from the GitHub Actions runner process. A malicious payload was staged in a GitHub fork via an orphaned commit, injected into published npm tarballs, then the legitimate TanStack/router workflow was hijacked to publish compromised versions with valid SLSA Build Level 3 provenance attestations. This is the first documented npm worm that produces validly attested malicious packages — meaning standard supply chain trust verification tools would pass these packages as legitimate.

The worm's self-propagation mechanism is what makes this wave different from prior Shai-Hulud attacks. Once installed, the malware locates any publishable npm token with bypass_2fa set to true, enumerates every package published by the same maintainer, and exchanges a GitHub OIDC token for a per-package publish token — sidestepping authentication entirely. The worm then spreads itself to every package that maintainer owns. That's how UiPath, DraftLab, and others got hit without being directly targeted — the worm spread from TanStack through shared maintainer identity chains.

The credential stealer targets cloud providers, cryptocurrency wallets, AI tools, messaging apps, and CI systems including GitHub Actions. Stolen data is exfiltrated to filev2.getsession[.]org — a Session Protocol infrastructure domain chosen specifically because decentralized privacy messaging services are unlikely to be blocked in enterprise environments. Fallback exfiltration commits encrypted data to attacker-controlled repositories using the author identity "claude@users.noreply.github.com" via the GitHub GraphQL API — the same Claude Code impersonation tactic used in the PyTorch Lightning wave documented in Global-Watch #001.

The malware establishes persistence hooks in both Claude Code and VS Code to survive reboots and re-execute the stealer on every IDE launch. A gh-token-monitor service polls api.github.com/user every 60 seconds to check if the npm token created by the malware has been revoked. If it detects revocation, a dead-man's switch triggers rm -rf ~/ — wiping the developer's home directory. The token is labelled "IfYouRevokeThisTokenItWillWipeTheComputerOfTheOwner." Developers must not revoke the token before isolating and imaging the system.

The Mistral AI and Guardrails AI PyPI compromises follow the earlier preinstall hook approach. The guardrails-ai package executes on import — not install — downloading a remote Python artifact from https://git-tanstack.com/transformers.pyz and executing it without integrity verification. Microsoft's analysis of the malicious mistralai package identified country-aware logic skipping Russian-language environments and a geofenced destructive branch with a 1-in-6 chance of executing rm -rf / when the system appears to be in Israel or Iran.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-45321, CVSS 9.6 — TanStack supply chain compromise
- **Campaign string:** "Shai-Hulud: Here We Go Again" — 400 attacker-controlled repos created
- **Attack chain:** pull_request_target trigger → GitHub Actions cache poisoning → OIDC token memory extraction from runner process
- *Orphaned commit staged malicious payload in GitHub fork → injected into npm tarballs → legitimate TanStack/router workflow hijacked for publishing*
- *SLSA Build Level 3 provenance bypass — first documented npm worm with valid provenance attestations*
- **Worm propagation:** locates bypass_2fa npm tokens → enumerates maintainer packages → exchanges OIDC token for per-package publish token → spreads autonomously
- **Malicious file:** router_init.js — obfuscated JS credential stealer
- **Persistence:** hooks into Claude Code and VS Code — re-executes stealer on every IDE launch
- Dead-man's switch: gh-token-monitor polls GitHub every 60 seconds; token revocation triggers rm -rf ~/
- **Token label:** "IfYouRevokeThisTokenItWillWipeTheComputerOfTheOwner"
- **Exfiltration:** filev2.getsession[.]org (Session Protocol infrastructure); fallback to attacker-controlled GitHub repos via GraphQL API
- **Fallback commit author identity:** claude@users.noreply.github.com — Claude Code impersonation
- **Malicious GitHub Actions workflows:** serialize repo secrets to JSON → upload to api.masscan[.]cloud
- **Guardrails AI:** executes on import; downloads from git-tanstack.com/transformers.pyz
- **Mistral AI (PyPI):** Russian locale skip; geofenced destructive branch — 1-in-6 rm -rf / on Israel/Iran systems
- **Affected:** 170+ packages, 518M+ cumulative downloads, npm and PyPI
- **Packages confirmed:** @tanstack/* (42 packages, 84 versions), guardrails-ai@0.10.1, mistralai@2.4.6, @opensearch-project/opensearch@3.5.3/3.6.2/3.7.0/3.8.0, @squawk/mcp@0.9.5, and others

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1195.001 — Supply Chain Compromise: Compromise Software Dependencies and Development Tools: GitHub Actions OIDC token hijacked via cache poisoning and memory extraction to publish malicious npm packages through legitimate TanStack workflow

- **TA0002 — Execution**
  - T1059.007 — Command and Scripting Interpreter: JavaScript: router_init.js obfuscated credential stealer executes via Bun runtime
  - T1059.006 — Command and Scripting Interpreter: Python: guardrails-ai executes Python artifact on import without integrity verification

- **TA0003 — Persistence**
  - T1546 — Event Triggered Execution: persistence hooks installed in Claude Code and VS Code — stealer re-executes on every IDE launch
  - T1053 — Scheduled Task/Job: gh-token-monitor service polls GitHub API every 60 seconds

- **TA0005 — Defense Evasion**
  - T1553.002 — Subvert Trust Controls: Code Signing: SLSA Build Level 3 provenance attestations forged — malicious packages appear legitimately signed
  - T1027 — Obfuscated Files or Information: router_init.js heavily obfuscated; payload disguised as legitimate transformers.pyz
  - T1036 — Masquerading: fallback commits use claude@users.noreply.github.com identity; token label disguised as warning
  - T1497 — Virtualization/Sandbox Evasion: Russian locale skip; Israel/Iran geofencing logic

- **TA0006 — Credential Access**
  - T1552.001 — Unsecured Credentials: Credentials in Files: cloud credentials, crypto wallets, AI tool configs, CI/CD secrets harvested from developer environments
  - T1528 — Steal Application Access Token: GitHub OIDC tokens extracted from Actions runner memory; npm publish tokens exchanged via OIDC

- **TA0009 — Collection**
  - T1005 — Data from Local System: comprehensive developer environment sweep — cloud creds, wallets, messaging apps, CI secrets
  - T1113 — Screen Capture: credential stealer collects developer environment data

- **TA0010 — Exfiltration**
  - T1567.001 — Exfiltration Over Web Service: Code Repositories: encrypted data committed to attacker-controlled GitHub repos via GraphQL API
  - T1071.001 — Application Layer Protocol: Web Protocols: exfiltration to filev2.getsession[.]org via Session Protocol infrastructure; api.masscan[.]cloud via injected Actions workflows

- **TA0040 — Impact**
  - T1485 — Data Destruction: dead-man's switch triggers rm -rf ~/ on token revocation — full home directory wipe
  - T1496 — Resource Hijacking: malicious GitHub Actions workflows inject into victim repos to serialize and exfiltrate CI/CD secrets

---

## Strategic Context

TeamPCP adding a dead-man's switch is the clearest signal yet that this group is escalating deliberately. Prior waves stole credentials and moved on. This wave threatens to destroy a developer's entire home directory if they try to remediate without following the correct procedure. That's coercive — it's designed to make defenders hesitate and to punish anyone who acts without knowing the wiper is there.

The SLSA Build Level 3 bypass is the most operationally significant technical development. SLSA was designed specifically to provide provenance guarantees for software packages — the assurance that what you're installing was built by the pipeline it claims and wasn't tampered with. TeamPCP bypassed that guarantee by hijacking the legitimate pipeline itself. If the pipeline is compromised, provenance attestations mean nothing. This breaks a fundamental assumption in the current supply chain security model.

This connects to Global-Watch #001 — the PyTorch Lightning/Intercom wave — as the same campaign. The Claude Code impersonation in fallback commits is consistent across both waves. TeamPCP is not just iterating tactically, they're building a durable worm infrastructure that spreads through maintainer identity chains autonomously. Each wave reaches a new set of ecosystems without requiring TeamPCP to manually target each one.

The geofenced destructive branch targeting Israel and Iran in the Mistral AI package is a separate signal worth tracking. Combined with the Russian locale skip, TeamPCP is embedding geopolitical targeting logic directly into their payloads. That's not financially motivated — that's ideological or state-adjacent behavior.

---

## Russian Language Context

The Mistral AI PyPI package contains country-aware logic that skips execution on Russian-locale systems — consistent across every documented Shai-Hulud wave. The string "Shai-Hulud: Here We Go Again" embedded across 400 attacker repositories continues the Dune-themed naming convention. The deliberate and consistent exclusion of Russian-language environments across six months of TeamPCP activity remains the strongest operational signal pointing toward Russian-ecosystem origin, though attribution is unconfirmed.

---

# Incident #018 — May 15, 2026

**Target:** Organizations running Cisco Catalyst SD-WAN Controller and SD-WAN Manager exposed to the internet

**Sector:** Network Infrastructure / Enterprise / Government

**Threat Actor:** Unknown — no attribution as of writing

**Origin:** Unknown

**Source:** The Hacker News — May 14, 2026 | Rapid7 (Jonah Burgess, Stephen Fewer)

**Attack Type:** Authentication Bypass → Privileged Access → Network Configuration Manipulation

**Labels:** Cisco | SD-WAN | CVE-2026-20182 | CVSS 10.0 | Auth Bypass | NETCONF | vdaemon | DTLS | Active Exploitation | Critical

---

## Analysis

Cisco disclosed CVE-2026-20182 on May 14, 2026 — a CVSS 10.0 authentication bypass in Cisco Catalyst SD-WAN Controller (formerly vSmart) and SD-WAN Manager (formerly vManage). Limited active exploitation was confirmed in May 2026. Cisco released patches and is urging immediate application.

The flaw is in the peering authentication mechanism. An unauthenticated remote attacker can send crafted requests to the vdaemon service over DTLS on UDP port 12346 and bypass authentication entirely. Successful exploitation logs the attacker in as an internal high-privileged non-root user account. From there they can access NETCONF and directly manipulate SD-WAN fabric configuration — routing, tunnels, policies across the entire network.

Rapid7, who discovered the vulnerability, noted this is not a patch bypass of CVE-2026-20127 — another CVSS 10.0 auth bypass in the same component that was exploited by UAT-8616 since at least 2023. Both hit the vdaemon service over the same DTLS port but are distinct issues in a similar part of the networking stack. The end result is identical though — unauthenticated remote attacker becomes an authenticated peer with privileged access.

Affected deployments include on-premises, Cisco SD-WAN Cloud-Pro, Cisco SD-WAN Cloud (Cisco Managed), and Cisco SD-WAN for Government (FedRAMP). The FedRAMP deployment being in scope makes this relevant beyond enterprise — US government networks using this product are directly in the blast radius.

---

## Key Technical Indicators:
- **CVE:** CVE-2026-20182 — CVSS 10.0
- **Vulnerable service:** vdaemon over DTLS, UDP port 12346
- **Attack vector:** crafted requests to peering authentication mechanism — no credentials required
- **Post-exploitation access:** internal high-privileged non-root user account
- **Post-exploitation capability:** NETCONF access — full SD-WAN fabric configuration manipulation
- **Related CVE:** CVE-2026-20127 (CVSS 10.0) — same service, same port, different issue — exploited by UAT-8616 since 2023
- **Affected deployments:** on-prem, SD-WAN Cloud-Pro, SD-WAN Cloud (Cisco Managed), SD-WAN for Government (FedRAMP)
- **Detection:** audit /var/log/auth.log for accepted publickey entries from unknown IPs for vmanage-admin
- **Detection:** check for unauthorized peer connections at unexpected times from unrecognized IPs
- **Exploitation status:** limited active exploitation confirmed May 2026

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: crafted requests sent to vdaemon service over DTLS UDP port 12346, exploiting a flaw in the peering authentication mechanism to bypass authentication entirely

- **TA0004 — Privilege Escalation**
  - T1078.003 — Valid Accounts: Local Accounts: successful bypass logs attacker in as an internal high-privileged non-root local user account on the SD-WAN Controller

- **TA0005 — Defense Evasion**
  - T1078 — Valid Accounts: attacker is treated as a legitimate authenticated peer by the system after bypass — no anomalous authentication events generated

- **TA0040 — Impact**
  - T1565.003 — Data Manipulation: Runtime Data Manipulation: NETCONF access enables direct manipulation of SD-WAN fabric configuration — routing, tunnels, and network policies across the managed environment

---

## Strategic Context

This is the second CVSS 10.0 auth bypass hitting the exact same service in Cisco's SD-WAN stack in a short period. CVE-2026-20127 was already being exploited by UAT-8616 since 2023 — and now a new, distinct issue in the same vdaemon networking stack is actively being exploited again. That pattern matters. When a specific component keeps producing critical flaws, it suggests either architectural weaknesses in that part of the codebase or that attackers are actively researching it.

SD-WAN controllers are high-value targets. Compromising one doesn't just give you access to a single device — it gives you visibility and control over the entire network fabric it manages. Routing, tunnels, policies — all of it. That makes this a network-level compromise, not just a server compromise.

The FedRAMP deployment being affected is worth noting. US government agencies using Cisco SD-WAN for Government are in scope. Combined with active exploitation already confirmed, this should have been patched the same day the advisory dropped.

---

# Incident #019 — May 18, 2026

**Target:** Unnamed open-source web-based system administration tool — planned mass exploitation of its users

**Sector:** Global / Cross-Sector / Open Source Infrastructure

**Threat Actor:** Unnamed prominent cybercrime group — no attribution disclosed by Google

**Origin:** Unknown — criminal, not state-sponsored

**Source:** Google Threat Intelligence Group (GTIG) — May 11, 2026 | The Hacker News | The Register

**Attack Type:** AI-Assisted Zero-Day Discovery — 2FA Bypass — Planned Mass Exploitation (Disrupted)

**Labels:** GTIG | AI-Generated Exploit | Zero-Day | 2FA Bypass | Python | OpenClaw | Mass Exploitation | Disrupted | Criminal | Google | Mandiant

---

## Analysis

On May 11, 2026, Google's Threat Intelligence Group published what is likely the most significant shift in the offensive threat landscape since the emergence of ransomware-as-a-service: **the first confirmed case of a criminal group using AI to develop a functional zero-day exploit**, intended for mass exploitation. Google disrupted the campaign by working with the affected vendor to patch the vulnerability before the attack launched.

The target was a popular open-source, web-based system administration tool — not named by Google. The exploit was a Python script designed to bypass two-factor authentication by exploiting a hardcoded trust exception in the tool's authentication logic. The flaw itself was a high-level semantic logic error — the kind that appears functionally correct to traditional scanners but is strategically broken from a security standpoint. The developer had hard-coded a trust assumption into the authentication flow, and an AI model found it.

GTIG assessed with high confidence that an AI model was used to both discover and weaponize the vulnerability. **The evidence is in the script itself:** an abundance of educational docstrings, a hallucinated CVSS score, and a structured textbook Python format consistent with LLM-generated code. The model used was not Gemini. OpenClaw is the working hypothesis. Google noted that the mistakes present in the exploit's implementation likely interfered with the attackers' plans even before GTIG intervened — the AI got them to a functional exploit but not a clean one.

The broader GTIG report published alongside this finding documents an ecosystem of AI-assisted offensive operations across both criminal and state-sponsored actors. APT45 (North Korea) is sending thousands of repetitive prompts to AI models to analyze CVEs and validate proof-of-concept exploits at industrial scale. Chinese state-linked operators are experimenting with AI for vulnerability hunting and automated target probing. Russian actors are using AI to pad malware with junk code to confuse analysts and AI-generated audio to manipulate news footage. A separate Android backdoor called Promptspy integrates Gemini APIs directly into live malware to autonomously navigate infected devices and replay authentication gestures.

The 2FA bypass case is one data point in a much larger pattern. The barrier to developing a functional zero-day just dropped. What previously required years of expertise in reverse engineering and exploit development can now be compressed into hours with the right model and the right target.

---

## Key Technical Indicators:
- **Exploit type:** zero-day 2FA bypass — implemented as a Python script
- **Target:** unnamed popular open-source web-based system administration tool
- **Vulnerability class:** high-level semantic logic flaw — hardcoded trust assumption in authentication flow
- **Exploitation requirement:** valid user credentials required before bypass applies
- **AI involvement:** GTIG high-confidence assessment — educational docstrings, hallucinated CVSS score, structured LLM-style Python format
- **AI model used:** not Gemini, not Anthropic Mythos — OpenClaw is working hypothesis
- **Status:** patched before mass exploitation — Google coordinated responsible disclosure with vendor
- **Related activity:** APT45 using AI for bulk CVE analysis; Chinese operators using AI for vulnerability hunting; Russian actors using AI for malware obfuscation and influence operations
- **Additional finding:** Promptspy Android backdoor — integrates Gemini API for autonomous device navigation and authentication gesture replay
- **Report date:** May 11, 2026

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: AI-generated Python exploit targeting a zero-day 2FA bypass in a public-facing open-source web administration tool
- **TA0006 — Credential Access**
  - T1111 — Multi-Factor Authentication Interception: exploit specifically designed to bypass two-factor authentication using a hardcoded trust flaw in the authentication logic
- **TA0042 — Resource Development**
  - T1587.001 — Develop Capabilities: Malware: AI model used to discover the vulnerability and generate a functional Python exploit — first confirmed AI-assisted zero-day development by a criminal actor
- **TA0005 — Defense Evasion**
  - T1027 — Obfuscated Files or Information: separate finding — Russian actors using AI-generated junk code inserted into malware to increase analyst evasion difficulty

---

## Strategic Context

**John Hultquist, chief analyst at GTIG**, put it directly: "There's a misconception that the race to AI vulnerabilities is imminent. The reality is it has already started." This is not a future threat. It happened in May 2026 and was only stopped because Google caught it early and the exploit had implementation errors.

The implications are straightforward. AI models are good at one specific thing that traditional scanners are not — reading code the way a developer does, understanding intent, and finding the gap between what a developer meant to do and what they actually implemented. Hardcoded trust assumptions, logic errors in authentication flows, subtle semantic mistakes — these are exactly what LLMs surface that fuzzing and static analysis miss.

The CVSS score being hallucinated is actually useful information from a threat intelligence standpoint. It tells you the model was not fed a database of known vulnerabilities — it was analyzing source code or documentation cold, generating its own severity assessment, and getting it wrong in the way LLMs get things wrong. The rest of the exploit was functional enough that a mass exploitation campaign was planned around it.

The broader GTIG report makes clear this is already an ecosystem, not an experiment. State actors and criminal groups are both in this space simultaneously, at different levels of sophistication. The compression of timelines between vulnerability discovery, weaponization, and exploitation is real and accelerating. Defenders who are not already integrating AI into their detection and response workflows are falling behind.

---

# Incident #020 — May 21, 2026

**Target:** Government agencies, IT services, aerospace, and electric power sector organizations across Russia, Georgia, Mongolia, Central Asia, and European government entities in Belgium, Italy, Serbia, Poland, and Spain

**Sector:** Government / IT Services / Aerospace / Energy / Education

**Threat Actor:** Webworm (overlaps with FishMonger/Aquatic Panda, SixLittleMonkeys, Space Pirates) — China-aligned

**Origin:** China-aligned — espionage motivated, active since at least 2022

**Source:** The Hacker News — May 20, 2026 

**Attack Type:** Custom Backdoor Deployment — Living-off-the-Land C2 via Discord and Microsoft Graph API

**Labels:** Webworm | EchoCreep | GraphWorm | China-aligned | Discord C2 | MS Graph API | SoftEther VPN | BadIIS | FishMonger | Space Pirates | Custom Backdoors | Espionage

---

## Analysis

ESET published research on May 20, 2026 documenting fresh Webworm activity from 2025, where the group added two new custom backdoors — EchoCreep and GraphWorm — to an already evolving toolkit. Both backdoors use legitimate **third-party platforms for C2:** EchoCreep routes commands through Discord, while GraphWorm uses Microsoft Graph API and OneDrive for file transfer and command execution. The earliest recorded EchoCreep C2 command dates to March 21, 2024, with 433 Discord messages sent via the channel to date.

EchoCreep handles basic operations — file upload and download, command execution via cmd.exe. **GraphWorm is more capable:** it can spawn new cmd.exe sessions, create and execute processes, transfer files through OneDrive, and terminate itself on operator signal. Both backdoors are a deliberate shift away from the older RATs Webworm previously relied on — Trochilus, Gh0st RAT, and 9002 RAT appear to have been abandoned.

The group stages malware through a GitHub repository disguised as a WordPress fork (github[.]com/anjsdgasdf/WordPress), and uses SoftEther VPN alongside custom proxy tools — WormFrp, ChainWorm, SmuxProxy, and WormSocket — to tunnel traffic and cover lateral movement. WormFrp retrieves its configuration from a compromised Amazon S3 bucket. These proxy tools encrypt communications and support chaining across multiple internal and external hosts. Initial access vectors are still unknown, but Webworm uses dirsearch and nuclei for web server enumeration and vulnerability scanning post-access.

The article also covers a separate but related development: Cisco Talos documented a BadIIS variant being sold under a MaaS model by a threat actor using the alias "lwxat," active since at least September 2021. BadIIS is shared across multiple Chinese-speaking cybercrime groups and supports traffic redirection, reverse proxying, content hijacking, and SEO fraud via a dedicated builder tool.

---

## Key Technical Indicators:
- **New backdoors:** EchoCreep (Discord C2), GraphWorm (Microsoft Graph API / OneDrive C2)
- **EchoCreep capabilities:** file upload/download, cmd.exe command execution
- **GraphWorm capabilities:** spawn cmd.exe sessions, process creation/execution, OneDrive file transfer, self-termination on operator signal
- **First EchoCreep C2 activity:** March 21, 2024 — 433 Discord messages sent via C2 channel
- **Malware staging:** GitHub repo impersonating WordPress fork — github[.]com/anjsdgasdf/WordPress
- **VPN tool:** SoftEther VPN — used across multiple Chinese threat actor clusters
- **Custom proxy tools:** WormFrp (S3 config retrieval), ChainWorm, SmuxProxy, WormSocket
- **Reconnaissance tools:** dirsearch, nuclei — web server file brute-forcing and vulnerability scanning
- **Abandoned tools:** Trochilus RAT, 9002 RAT (Hydraq/McRat)
- **Actor overlaps:** FishMonger/Aquatic Panda, SixLittleMonkeys, Space Pirates
- **BadIIS MaaS operator alias:** lwxat — builder tool active since September 2021
- **Initial access vector:** unknown as of report date

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: dirsearch and nuclei used to brute-force web server directories and scan for exploitable vulnerabilities as likely precursor to access
- **TA0002 — Execution**
  - T1059.003 — Command and Scripting Interpreter: Windows Command Shell: both EchoCreep and GraphWorm execute commands via cmd.exe sessions on compromised hosts
- **TA0003 — Persistence**
  - T1505.003 — Server Software Component: Web Shell: BadIIS deployed as a persistent IIS server component with service-based installers surviving server restarts
- **TA0005 — Defense Evasion**
  - T1102.002 — Web Service: Bidirectional Communication: EchoCreep routes C2 through Discord; GraphWorm routes C2 through Microsoft Graph API — both blend into legitimate cloud traffic
  - T1036.005 — Masquerading: Match Legitimate Name or Location: malware staged on GitHub repository disguised as a WordPress fork
  - T1090.003 — Proxy: Multi-hop Proxy: WormFrp, ChainWorm, SmuxProxy, and WormSocket chain traffic across multiple internal and external hosts to obscure operator infrastructure
- **TA0009 — Collection**
  - T1005 — Data from Local System: GraphWorm uploads files from compromised hosts to OneDrive via Microsoft Graph API
- **TA0011 — Command and Control**
  - T1071.001 — Application Layer Protocol: Web Protocols: GraphWorm uses Microsoft Graph API over HTTPS as C2 channel
  - T1219 — Remote Access Software: SoftEther VPN used alongside proxy tools to tunnel operator traffic and maintain persistent remote access

---

## Strategic Context

The shift to Discord and Microsoft Graph API for C2 is becoming a pattern across Chinese APT clusters — it's not unique to Webworm. Legitimate cloud services generate massive volumes of traffic, blend into normal enterprise activity, and are rarely blocked at the network boundary. Traditional C2 detection based on known malicious IPs or domains becomes much less effective when the traffic looks identical to an employee using OneDrive or a developer using Discord.

The abandonment of Trochilus and 9002 RAT in favour of custom tools is worth noting. Older RATs have known signatures and detection rules. Building new tooling specifically designed around legitimate API abuse is a deliberate evasion investment.

Russia appearing in the target scope alongside European government entities reflects Webworm's broad tasking — this isn't a single-campaign actor. The Space Pirates overlap is particularly relevant here given Space Pirates' documented focus on Russian IT firms and aerospace. The targeting profile across both clusters suggests consistent Chinese intelligence collection priorities against Russian strategic sectors, which is a notable dynamic in the context of the Sino-Russian relationship.

---

## Russian Language Context

Webworm's targeting of российские государственные организации (rossiyskiye gosudarstvennyye organizatsii) — Russian government organizations — and аэрокосмический сектор (aerokosmicheskiy sektor) — the aerospace sector — reflects persistent Chinese intelligence collection against стратегические отрасли (strategicheskiye otrasli) — strategic industries. The use of легитимные облачные сервисы (legitimnyye oblachnyye servisy) — legitimate cloud services — such as Discord and Microsoft Graph API for управление и контроль (upravleniye i kontrol') — command and control — represents a growing evasion trend observed across several Chinese-nexus clusters operating against Russian targets.

---

# Incident #021 — May 22, 2026

**Target:** Telecommunications provider in the Middle East; ISP in Afghanistan; unknown entity in Azerbaijan; possible compromises in the US and Ukraine

**Sector:** Telecommunications / ISP / Critical Infrastructure

**Threat Actor:** Calypso (aka Bronze Medley, Red Lamassu) — China-aligned, active since at least September 2016

**Origin:** China — C2 infrastructure geolocated to Chengdu, Sichuan — state-sponsored espionage

**Source:** The Hacker News — May 21, 2026 | Lumen Technologies Black Lotus Labs (Danny Adamitis) | PwC

**Attack Type:** Linux Modular Backdoor — SOCKS5 Proxy — LAN Pivoting — Long-Term Persistent Access

**Labels:** Calypso | Red Lamassu | Showboat | JFMBackdoor | EvaRAT | Linux | SOCKS5 | DLL Side-Loading | PlugX | Chengdu | Telecom | Afghanistan | Middle East | China-aligned

---

## Analysis

Lumen Technologies Black Lotus Labs published research on May 21, 2026 documenting Showboat, a previously undisclosed Linux malware family used by Calypso against a telecommunications provider in the Middle East since at least mid-2022. The initial discovery came from an ELF binary uploaded to VirusTotal in May 2025 — Kaspersky tracks the same artifact as EvaRAT.

Showboat is a modular post-exploitation framework built for Linux systems. It contacts a C2 server, collects system information, and sends it back encoded as a Base64 and encrypted string inside a PNG field. From there it supports remote shell access, file upload and download, SOCKS5 proxy functionality, and the ability to scan for and connect to other LAN-accessible devices. It hides itself from the process list by retrieving a code snippet hosted on Pastebin — the paste was created January 11, 2022. The SOCKS5 proxy capability is the key design choice: it lets attackers reach internal machines that have no direct internet exposure.

Alongside Showboat, Calypso deployed JFMBackdoor — a Windows implant delivered via DLL side-loading — against the Afghanistan telecom target. A batch script launches a legitimate executable which loads the rogue DLL. JFMBackdoor supports remote shell, file operations, network proxying, screenshots, and self-removal.

Infrastructure analysis linked C2 nodes to IP addresses geolocated in Chengdu. A secondary C2 cluster using similar X.509 certificates pointed to compromises in Afghanistan and Azerbaijan, with two possible US victims and one in Ukraine. Initial access vector for Showboat is currently unknown, though Calypso has historically used ASPX web shells after exploiting public-facing vulnerabilities or default credentials.

Calypso shares tooling overlap with SixLittleMonkeys and Webworm — both documented in recent entries — and sits within the broader Chinese digital quartermaster ecosystem alongside PlugX, ShadowPad, and NosyDoor.

---

## Key Technical Indicators:
- **Malware:** Showboat (Linux) — also tracked as EvaRAT by Kaspersky
- **First VirusTotal upload:** May 2025 — active since at least mid-2022
- **Showboat capabilities:** remote shell, file upload/download, SOCKS5 proxy, LAN device scanning, process list concealment
- **Stealth mechanism:** retrieves concealment code from Pastebin paste created January 11, 2022
- **C2 data format:** system info encrypted and Base64-encoded, exfiltrated inside PNG field
- **Windows implant:** JFMBackdoor — delivered via DLL side-loading via batch script + legitimate executable
- **JFMBackdoor capabilities:** remote shell, file ops, network proxying, screenshots, self-removal
- **C2 geolocation:** Chengdu, Sichuan, China
- **Confirmed victims:** Middle East telecom (primary), Afghanistan ISP, Azerbaijan (unknown entity)
- **Secondary cluster victims:** 2 possible US, 1 Ukraine
- **ELF SHA-256:** d6a4fad5448838dbc8cc6b33f1dbfbdc7a2fad36de58ff6a66dce96f729f7011
- **Actor overlaps:** SixLittleMonkeys, Webworm, Mikroceen cluster — shared digital quartermaster assessed
- **Initial access:** unknown — historically ASPX web shell via CVE exploitation or default credential abuse

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: Calypso historically exploits public-facing vulnerabilities (CVE-2021-26855 ProxyLogon) and default remote access credentials to deploy web shells as initial foothold
- **TA0002 — Execution**
  - T1574.002 — Hijack Execution Flow: DLL Side-Loading: JFMBackdoor delivered via batch script loading a rogue DLL through a legitimate executable
- **TA0003 — Persistence**
  - T1505.003 — Server Software Component: Web Shell: ASPX web shell used historically as persistent access mechanism following initial exploitation
- **TA0005 — Defense Evasion**
  - T1564.001 — Hide Artifacts: Hidden Files and Directories: Showboat conceals itself from the process list using a Pastebin-hosted code snippet
  - T1027 — Obfuscated Files or Information: C2 exfiltration data encrypted and Base64-encoded inside a PNG field to blend with normal traffic
- **TA0009 — Collection**
  - T1082 — System Information Discovery: Showboat collects and transmits full system information to C2 on initial contact
  - T1113 — Screen Capture: JFMBackdoor captures screenshots on compromised Windows hosts
- **TA0011 — Command and Control**
  - T1090.001 — Proxy: Internal Proxy: SOCKS5 proxy functionality enables C2 routing through compromised hosts to reach LAN-internal machines with no internet exposure
  - T1102 — Web Service: Showboat retrieves concealment code from a Pastebin paste as part of its evasion routine
- **TA0008 — Lateral Movement**
  - T1021 — Remote Services: Showboat scans for and connects to other LAN-accessible devices via SOCKS5 proxy, enabling movement to non-internet-exposed internal systems

---

## Strategic Context

Calypso has been quietly running Showboat against telecom infrastructure since mid-2022 — over three years of undetected access before public disclosure. Telecom providers are prime targets for this kind of long-term persistence because they sit at the centre of communications infrastructure. Compromising the network rather than individual endpoints gives access to traffic flows, subscriber data, and internal systems that would otherwise be completely out of reach.

The Ukraine victim in the secondary C2 cluster is worth flagging. Calypso's documented targeting includes Russia, which combined with the Ukraine compromise puts this actor in a position of collecting intelligence on both sides of the conflict. That is consistent with Chinese intelligence priorities — Beijing has strong interest in understanding how the war develops regardless of the outcome.

The Chengdu geolocation is also notable in context — Chengdu is home to documented Chinese state cyber operations infrastructure, and this isn't the first time C2 nodes have been traced there.

---

## Russian Language Context

Calypso's documented targeting of российские государственные учреждения (rossiyskiye gosudarstvennyye uchrezhdeniya) — Russian state institutions — alongside confirmed infrastructure in Украина (Ukraina) — Ukraine — places this actor in direct overlap with the Russo-Ukrainian conflict intelligence landscape. The use of прокси-сервер (proksi-server) — proxy server — specifically SOCKS5, for доступ к внутренней сети (dostup k vnutrenney seti) — access to the internal network — reflects a consistent Chinese operational pattern of deep, quiet persistence inside телекоммуникационная инфраструктура (telekommunikatsionnaya infrastruktura) — telecommunications infrastructure.

---

# Incident #022 — May 27, 2026

**Target:** Defense, aviation, and telecommunications organizations across the United States, Europe, and the Middle East; specific confirmed targeting of aerospace and software sector personnel

**Sector:** Defense / Aviation / Telecommunications / Software

**Threat Actor:** Nimbus Manticore (aka UNC1549, Screening Serpens, Smoke Sandstorm) — IRGC-affiliated Iranian state-sponsored APT

**Origin:** Iran — Islamic Revolutionary Guard Corps (IRGC) — state-sponsored espionage, wartime intelligence collection

**Source:** The Hacker News — May 26, 2026 | Check Point Research | Palo Alto Networks Unit 42 | Security Affairs

**Attack Type:** Three-Wave Campaign — Phishing + SEO Poisoning + Trojanized Installer → MiniFast/MiniJunk V2 Backdoor Deployment

**Labels:** Nimbus Manticore | UNC1549 | IRGC | MiniFast | MiniJunk | SEO Poisoning | AppDomain Hijacking | Trojanized Zoom | AI-Assisted Malware | Operation Epic Fury | Aviation | Defense | Wartime | Iran

---

## Analysis

Check Point Research published analysis on May 22-26, 2026 documenting three waves of Nimbus Manticore activity between February and April 2026, all coinciding with Operation Epic Fury — the US-Israeli military campaign against Iran launched on February 28, 2026. The group accelerated operations immediately following the strikes, introducing new techniques and tooling not previously observed.

The February wave used career-themed phishing emails delivering ZIP archives hosted on legitimate platforms like OnlyOffice, consistent with prior Nimbus Manticore tradecraft. The March wave introduced a trojanized Zoom installer as a delivery vehicle — victims were likely targeted via fake meeting invitations — which used AppDomain hijacking to deploy MiniFast, a previously undocumented backdoor. April marked the most significant shift: the group abandoned phishing entirely for a counterfeit Oracle SQL Developer download page, using SEO poisoning to push it into search results — the first time this technique has been observed from this actor. Check Point assessed that MiniFast shows signs of AI-assisted development based on excessive error handling, verbose and repetitive function naming, detailed debug-style status messages, and modular code structure inconsistent with the malware's overall simplicity.

MiniJunk V2, an evolved version of the MiniJunk framework documented in 2025, was also deployed alongside MiniFast. The group's historical toolchain includes Minibike (SlugResin), MiniBrowse credential stealer, and MiniJunk — all part of a consistent naming convention suggesting centralized internal development.

---

## Key Technical Indicators:
- **Campaign period:** February–April 2026 — three distinct waves tied to Operation Epic Fury
- **Wave 1 (Feb):** career-themed phishing — ZIP archives hosted on OnlyOffice and similar legitimate platforms
- **Wave 2 (Mar):** trojanized Zoom installer — fake meeting invitation lure — AppDomain hijacking to deploy MiniFast
- **Wave 3 (Apr):** SEO poisoning — fake Oracle SQL Developer download page — first observed use of this technique by actor
- **New backdoor:** MiniFast (aka MiniUpdate) — AI-assisted development assessed by Check Point
- **AI indicators in MiniFast:** excessive error handling, verbose function names, debug-style strings, modular structure despite simplicity
- **Updated tool:** MiniJunk V2 — evolved version of 2025 MiniJunk framework
- **Historical toolchain:** Minibike/SlugResin, MiniBrowse, MiniJunk — consistent internal naming convention
- **Prior technique replaced:** DLL side-loading → AppDomain hijacking
- **Attribution:** IRGC-affiliated — high confidence — Check Point, Unit 42, Mandiant (historical)

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.002 — Phishing: Spear-phishing Link: career-themed and meeting invitation phishing emails delivering malicious archives and trojanized installers
  - T1608.006 — Stage Capabilities: SEO Poisoning: counterfeit Oracle SQL Developer download page pushed into search results via SEO poisoning — first observed use by this actor
- **TA0002 — Execution**
  - T1204.002 — User Execution: Malicious File: victims execute trojanized Zoom installer or download page payload — triggering AppDomain hijacking chain
- **TA0003 — Persistence**
  - T1546 — Event Triggered Execution: AppDomain hijacking used to persist MiniFast execution within legitimate application domains — replacing previous DLL side-loading technique
- **TA0005 — Defense Evasion**
  - T1574.014 — Hijack Execution Flow: AppDomain Hijacking: malicious code injected into .NET application domains to execute MiniFast while appearing as legitimate software activity
  - T1036.005 — Masquerading: Match Legitimate Name or Location: trojanized Zoom installer and fake SQL Developer download page masquerade as legitimate software
- **TA0042 — Resource Development**
  - T1587.001 — Develop Capabilities: Malware: MiniFast developed with AI assistance — accelerating tooling development under wartime operational pressure

---

## Strategic Context

Three technique shifts in three months — SEO poisoning, AppDomain hijacking, AI-assisted malware — is a fast pace of evolution for any APT, let alone one operating under active wartime conditions. The timing is not coincidental. Operation Epic Fury created immediate intelligence collection pressure on IRGC-affiliated groups, and Nimbus Manticore responded by rapidly expanding its capability set rather than slowing down.

The move to SEO poisoning is the most significant tactical shift. Phishing requires a target to open an email. SEO poisoning intercepts anyone actively searching for legitimate software — a much broader and more passive attack surface. Combined with AI-assisted malware development compressing the time between concept and deployment, this group is operating at a pace that suggests either significant resource injection or deliberate wartime acceleration from IRGC leadership.

Europe's presence in the targeting scope alongside the US and Middle East reflects the group's interest in NATO-adjacent defense and aviation infrastructure during an active conflict involving a US-led coalition.
