# 🛡️ Threat Intelligence Log — May 2026

> Active monitoring of global cyber incidents.
> Focus: Russian-language threat actors and European infrastructure.
> Language access: Russian (A2→B2) | German (planned)

---

# Incident #001 — May 2, 2026

**Target:** Senior German government officials — including two cabinet members; Signal messaging app users in government

**Sector:** Government / Political / Diplomatic

**Threat Actor:** Russia — assessed; specific group unconfirmed

**Origin:** Russia — assessed by German government

**Source:** CYFIRMA Weekly Intelligence — May 1, 2026

**Attack Type:** Phishing via Signal — Device Compromise / Data Exfiltration

**Labels:** Russia | Germany | Signal | Phishing | Cabinet | Government Targeting | Hybrid Warfare | Data Exfiltration | Berlin

---

## Analysis

Germany's government confirmed a cyberattack targeting senior officials in Berlin through the Signal messaging app. Two cabinet members were among those targeted. Affected individuals were notified and the data leak from compromised devices was described as contained, though authorities could not rule out additional victims. The German government attributed the attack to Russia, consistent with its broader assessment that Russia represents the biggest threat to German national security and is conducting active hybrid warfare operations.

Signal is increasingly used by government officials as a secure alternative to standard messaging and email. Its adoption as a phishing target signals that threat actors are following their targets wherever sensitive communications migrate — secure platform reputation does not translate to immunity from social engineering.

---

## Key Technical Indicators:
- **Platform targeted:** Signal messaging app
- **Targets:** senior German government officials — two cabinet members confirmed
- **Attack type:** phishing campaign via Signal
- **Data leak:** confirmed from compromised devices — described as contained
- **Additional victims:** cannot be ruled out per German authorities
- **Attribution:** Russia — assessed by German government; specific group unconfirmed
- *Official spoke on condition of anonymity per government protocol*

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566 — Phishing: phishing campaign delivered via Signal messaging app targeting senior German government officials

- **TA0006 — Credential Access**
  - T1056 — Input Capture: device access or session data compromised via phishing; credentials and data extracted from affected devices

- **TA0009 — Collection**
  - T1114 — Email Collection: sensitive government communications collected from compromised official devices
  - T1636 — Contact List: device data including contacts and messages accessed via compromised Signal sessions

- **TA0010 — Exfiltration**
  - T1041 — Exfiltration Over C2 Channel: data exfiltrated from compromised devices before containment

---

## Strategic Context

Russia targeting German cabinet members via Signal is a continuation of documented hybrid warfare against Germany that has escalated through 2025 and into 2026. Germany has been one of the most vocal European supporters of Ukraine and has faced repeated Russian cyber operations as a result. Moving to Signal suggests Russian actors are adapting to target hardening — as government email security improves, attackers pivot to the next communication layer.

Two cabinet members targeted — not junior staff — indicates deliberate high-value targeting aimed at Germany's policy decision-making level, not opportunistic phishing.

---

## Russian Language Context

Германия (Germaniya) — Germany — has been a primary target of Russian hybrid warfare operations throughout 2025–2026, consistent with its role as a leading European supporter of Ukraine. 
The Signal phishing campaign targeting Berlin cabinet members fits the GRU and FSB doctrine of targeting высокопоставленные чиновники (vysokopostavlennyye chinovniki) — high-ranking officials — to gain insight into policy deliberations on Ukraine support and NATO coordination.

---

# Incident #002 — May 2, 2026

**Target:** Organizations globally — government, enterprise, civil society; SOHO router users

**Sector:** Government / Enterprise / Network Infrastructure

**Threat Actor:** APT28 (Fancy Bear / Pawn Storm) — GRU Unit 26165, Russian state-sponsored

**Origin:** Russia — GRU, high confidence

**Source:** UK NCSC Advisory — April 2026 | Dark Reading — May 1, 2026

**Attack Type:** SOHO Router Exploitation → Malicious DNS Server Deployment → AiTM Credential Theft

**Labels:** APT28 | FrostArmada | DNS Hijacking | SOHO Routers | AiTM | Credential Theft | OAuth | VPS | GRU | NCSC | Fancy Bear

---

## Analysis

The UK NCSC published an advisory detailing APT28's FrostArmada campaign — an ongoing router exploitation operation in which the group compromises SOHO routers and reconfigures their DHCP/DNS settings to point to attacker-controlled VPS servers operating as malicious DNS resolvers. Since 2024 and continuing into 2026, APT28 has been building two distinct VPS clusters identified by banner pattern analysis, each receiving high volumes of DNS requests from compromised routers.

The mechanism is straightforward but effective. Compromised routers — targeted via publicly known vulnerabilities — have their DHCP DNS server settings overwritten to include APT28-controlled IP addresses. Any device on that network that performs a DNS lookup gets a response from the attacker's server instead of a legitimate resolver. The malicious DNS server returns poisoned responses that redirect victims to attacker-controlled infrastructure, enabling adversary-in-the-middle attacks that harvest passwords, OAuth tokens, and session cookies for web and email services.

The campaign is assessed as opportunistic in its initial targeting — APT28 casts a wide net of compromised routers, then filters for victims of intelligence value at each subsequent stage. This is a volume-then-triage approach: compromise many, exploit selectively. The NCSC advisory identifies web and email service credentials as the primary collection objective, consistent with APT28's documented focus on accessing communications infrastructure for intelligence collection.

APT28's router exploitation methodology is not new — DNS hijacking via SOHO router compromise has been in their playbook for over two decades. Feike Hacquebord of TrendAI noted that the technique's persistence in APT28 operations reflects a simple reality: old methods that still work don't get retired.

---

## Key Technical Indicators:
- **Campaign:** FrostArmada — APT28 SOHO router DNS hijacking operation
- **Attack chain:** router exploitation via public CVEs → DHCP/DNS settings overwritten → VPS configured as malicious DNS server → AiTM credential theft
- *Two VPS banner pattern clusters identified by NCSC*
- *DHCP DNS settings modified to include APT28-controlled IPs*
- **Targeting:** opportunistic router compromise → selective victim filtering by intelligence value
- **Credentials targeted:** passwords, OAuth tokens, session cookies for web and email services
- *Active since 2024, confirmed ongoing into 2026*
- **Technique age:** DNS hijacking via SOHO routers — 20+ year APT28 methodology still in active use

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: SOHO routers exploited via publicly known vulnerabilities to gain access and modify DNS settings

- **TA0005 — Defense Evasion**
  - T1584.008 — Compromise Infrastructure: Botnet: compromised SOHO routers used as proxy infrastructure to obscure APT28 origin
  - T1090 — Proxy: VPS clusters operate as malicious DNS servers, routing victim traffic through attacker-controlled infrastructure

- **TA0006 — Credential Access**
  - T1557 — Adversary-in-the-Middle: malicious DNS responses redirect victims to attacker infrastructure enabling AiTM attacks
  - T1539 — Steal Web Session Cookie: OAuth tokens and session cookies harvested via AiTM position
  - T1110 — Brute Force: credentials for web and email services harvested via poisoned DNS resolutions

- **TA0007 — Discovery**
  - T1595 — Active Scanning: opportunistic router targeting followed by victim filtering based on intelligence value assessment

- **TA0011 — Command & Control**
  - T1583.002 — Acquire Infrastructure: Virtual Private Server: VPS infrastructure configured as malicious DNS resolvers
  - T1071 — Application Layer Protocol: DNS used as C2 and redirection mechanism

---

## Strategic Context

FrostArmada is APT28 operating at infrastructure level — not targeting specific organizations directly but poisoning the network layer that organizations rely on. DNS is foundational. When the resolver is compromised, every lookup is a potential collection opportunity regardless of what security controls the target organization has deployed at the application layer.

The volume-then-triage model is operationally efficient. APT28 doesn't need to know who it wants before it starts — it compromises broadly, then identifies targets of value from the credential stream. That's a fundamentally different threat model from targeted spearphishing and it requires a different defensive response: router firmware hygiene and DNS monitoring rather than email gateway controls.

This is the second APT28 entry in RU-Threats this month alongside #001 (Signal/Germany). APT28 is running at least three simultaneous campaign tracks in the May 2026 timeframe — FrostArmada router DNS hijacking, Prismex Windows Shell exploitation (#043 April), and the Signal phishing operation against German officials (#001 May). Multiple simultaneous campaigns across different attack surfaces is consistent with GRU operational doctrine of sustained, multi-vector pressure.

---

## Russian Language Context

APT28 is assessed as GRU Войсковая часть 26165 (Voyskavaya chast 26165) — Military Unit 26165, 85th Main Special Service Centre. The FrostArmada campaign's router exploitation methodology reflects устойчивый доступ (ustoychivy dostup) — persistent access — as a core GRU operational priority, consistent with long-term intelligence collection rather than disruptive operations.


---

# Incident #003 — May 12,  2026

**Target:** Western critical infrastructure — energy sector organizations, North American and European critical infrastructure providers, telecom companies, organizations with cloud-hosted network infrastructure

**Sector:** Critical Infrastructure / Energy / Telecommunications / Cloud

**Threat Actor:** APT44 (Sandworm / FROZENBARENTS / Seashell Blizzard / Voodoo Bear) — GRU Military Unit 74455, Russian state-sponsored

**Origin:** Russia — GRU MU74455, high confidence

**Source:** Amazon Web Services / Amazon Threat Intelligence — December 15, 2025 | The Hacker News — December 17, 2025

**Attack Type:** Misconfigured Edge Device Exploitation → Passive Traffic Interception → Credential Harvesting → Credential Replay

**Labels:** APT44 | Sandworm | Seashell Blizzard | GRU | Critical Infrastructure | Energy | Edge Device | Credential Theft | Credential Replay | AWS | North America | Europe | 2021-2025

---

## Analysis

Amazon Threat Intelligence disclosed a years-long APT44/Sandworm campaign targeting Western critical infrastructure that ran from 2021 through at least the end of 2025. The campaign is attributed to GRU Military Unit 74455 with high confidence based on infrastructure overlaps with known Sandworm operations in Amazon's telemetry. Targets included energy sector organizations across Western nations, critical infrastructure providers in North America and Europe, telecom companies, and organizations with cloud-hosted network infrastructure.

The tactical evolution is what makes this disclosure significant. Early in the campaign APT44 exploited known vulnerabilities — WatchGuard Firebox/XTM (CVE-2022-26318), Atlassian Confluence (CVE-2021-26084, CVE-2023-22518), and Veeam backup software. Over time, and particularly through 2024-2025, N-day and zero-day exploitation declined. The group shifted to targeting misconfigured customer network edge devices with exposed management interfaces — a simpler, lower-risk approach that achieves the same operational outcomes with less exposure. As Amazon's CISO CJ Moses noted, the compromises were not due to AWS weaknesses but to customers failing to properly secure their own network edge devices and virtual appliances.

The mechanism is straightforward. APT44 identifies internet-exposed network appliances with weak or default configurations and establishes persistent connections to compromised EC2 instances running customers' network appliance software. From that position — sitting on the network edge — the group conducts passive traffic interception to collect credentials in transit without ever touching individual endpoints. Those harvested credentials are then replayed against victim organizations' online services — cloud platforms, collaboration tools, backup systems — enabling lateral movement across the broader organization.

Amazon's telemetry showed actor-controlled IP addresses maintaining persistent connections to compromised EC2 instances, consistent with long-term interactive access and data retrieval across multiple affected instances. The scale and duration — five years, multiple sectors, North America and Europe — reflects a sustained strategic intelligence collection operation consistent with GRU doctrine, not opportunistic exploitation.

---

## Key Technical Indicators:
- **Campaign duration:** 2021–2025 — confirmed ongoing at time of disclosure
- **Attribution:** APT44 / Sandworm / GRU MU74455 — high confidence via infrastructure overlaps in Amazon telemetry
- **Early phase CVEs:** CVE-2022-26318 (WatchGuard Firebox/XTM), CVE-2021-26084 / CVE-2023-22518 (Atlassian Confluence), Veeam backup software vulnerabilities
- **Tactical shift:** N-day/zero-day exploitation declined 2024-2025; pivot to misconfigured network edge device exploitation
- **Attack surface:** customer-deployed network appliances with exposed management interfaces — routers, VPNs, management appliances
- **Method:** passive traffic interception from edge device position → credential harvesting in transit
- **Post-harvest:** credential replay against cloud services, collaboration platforms, backup systems
- **AWS telemetry:** actor-controlled IPs establishing persistent connections to compromised EC2 instances running customer network appliance software
- **Target sectors:** energy (primary), telecom, cloud-hosted infrastructure, North America and Europe
- **Disruption:** Amazon took steps to disrupt ongoing activity and notified affected customers

---

## MITRE ATT&CK Tactics and Techniques:

- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: early phase exploitation of WatchGuard, Confluence, Veeam CVEs; pivot to misconfigured edge device management interfaces
  - T1078.001 — Valid Accounts: Default Accounts: misconfigured network appliances with exposed management interfaces exploited via weak or default credentials

- **TA0003 — Persistence**
  - T1133 — External Remote Services: persistent connections maintained to compromised EC2 instances running customer network appliance software

- **TA0005 — Defense Evasion**
  - T1090 — Proxy: compromised edge devices used as passive interception points — no direct endpoint access required; activity blends into legitimate network traffic

- **TA0006 — Credential Access**
  - T1557 — Adversary-in-the-Middle: passive traffic interception from edge device position captures credentials in transit
  - T1110.004 — Brute Force: Credential Stuffing: harvested credentials replayed against victim organizations' online services

- **TA0007 — Discovery**
  - T1595.002 — Active Scanning: Vulnerability Scanning: identification of internet-exposed network appliances with weak configurations

- **TA0008 — Lateral Movement**
  - T1078 — Valid Accounts: replayed credentials used to access cloud platforms, collaboration tools, and backup systems across victim organizations

- **TA0009 — Collection**
  - T1040 — Network Sniffing: passive traffic interception on compromised edge devices captures authentication traffic and credentials in transit

- **TA0011 — Command & Control**
  - T1071 — Application Layer Protocol: persistent connections to compromised EC2 instances via application layer protocols

---

## Strategic Context

APT44 shifting away from CVE exploitation toward misconfigured edge device abuse is a deliberate tactical evolution. Patching fixes CVEs. It doesn't fix misconfigured management interfaces that have been exposed to the internet for years. By moving to that attack surface, Sandworm reduced its operational exposure — no need to develop or purchase exploits, no patch cycle to race against — while maintaining the same access outcomes. That's a mature threat actor adapting to a defensive environment that got better at patching.

The edge device positioning for passive credential interception is the most operationally elegant aspect of this campaign. Sitting on the network edge means every authentication attempt that passes through the device is visible. No malware on endpoints, no lateral movement needed to find credentials — the credentials come to you. Then credential replay against cloud services completes the access chain without ever touching the internal network directly.

This connects directly to RU-Threats #002 — APT28 FrostArmada running a parallel router DNS hijacking campaign during the same period. Two separate GRU-affiliated operations targeting network edge infrastructure simultaneously — APT44 for credential interception at scale, APT28 for AiTM credential theft via DNS poisoning. Edge devices are the GRU's preferred persistent access layer in 2025-2026.

---

## Russian Language Context

APT44 operates as Войсковая часть 74455 (Voyskavaya chast 74455) — GRU Military Unit 74455, 85th Main Special Service Centre's subordinate unit. The campaign's focus on Western энергетическая инфраструктура (energeticheskaya infrastruktura) — energy infrastructure — is consistent with GRU strategic priorities around understanding and potentially disrupting Western energy supply chains, particularly in the context of ongoing geopolitical pressure related to Ukraine support.

---

# ⚠️ Cross-Sector Alert: TeamPCP — Persistent Russian Locale Evasion Pattern
**Status:** Active — Mini Shai-Hulud latest wave, May 12, 2026  
**Target:** npm and PyPI developers globally — 170+ packages, 518M cumulative downloads  
**Note:** Every TeamPCP/Shai-Hulud wave consistently skips Russian-locale systems. Latest wave adds geofenced destructive branch targeting Israel and Iran. Russian-ecosystem origin assessed but **unconfirmed across six months of activity.**

👉 **[Read Full Entry in Global-Watch](../Global-Watch/MAY-2026-LOG.md#incident-017)**

---

# Incident #004 — May 18, 2026

**Target:** Government, diplomatic, and defense organizations across Europe, Central Asia, and Ukraine

**Sector:** Government / Defense / Diplomatic

**Threat Actor:** Secret Blizzard (aka Turla, Venomous Bear, Uroburos, Snake, Blue Python, WRAITH, ATG26)

**Origin:** Russia — FSB Center 16 — state-sponsored espionage

**Source:** Microsoft Threat Intelligence — May 14, 2026

**Attack Type:** Modular P2P Botnet — Long-Term Persistent Access — Intelligence Collection

**Labels:** Secret Blizzard | Turla | Kazuar | FSB | P2P Botnet | Modular Malware | Pelmeni | ShadowLoader | Espionage | Europe | Ukraine | Long-Term Access

---

## Analysis

Microsoft Threat Intelligence published a deep technical breakdown of Kazuar on May 14, 2026, documenting its evolution from a standard .NET backdoor into a fully modular peer-to-peer botnet. Kazuar has been in active development since at least 2017, with code lineage traced back to 2005. This is not a new tool — it is a continuously upgraded one, and that distinction matters. Secret Blizzard has been quietly rebuilding it for years while most defenders treated it as a known and understood threat.

The architecture is now split across three distinct module types: Kernel, Bridge, and Worker. The Kernel acts as the central coordinator — it issues tasks, manages C2 communication through the Bridge, and handles internal logging. The Bridge is the external communications layer, proxying traffic between the Kernel and the C2 server using HTTP, WebSockets, or Exchange Web Services. The Worker handles actual collection — keylogging, screenshots, file harvesting, email content, browser data, running processes, USB devices, and more.

The most significant design choice is the leader election mechanism. Out of all infected machines in a network, only one Kernel module is elected as the active leader at any given time. Only that elected leader communicates externally. Every other infected machine stays silent internally. The result is a botnet that generates almost no suspicious outbound traffic across a compromised network — a single machine talking to C2 while dozens of others collect quietly.

Delivery happens through the Pelmeni dropper, which embeds an encrypted payload inside itself. In some cases the payload is bound to the target's specific hostname, meaning it will only decrypt and execute on the exact machine it was built for — making early detection much harder. ShadowLoader is used as an alternative delivery mechanism.

Inter-module communication uses AES-encrypted messages serialized with Google Protocol Buffers over Windows Messaging, Mailslots, or Named Pipes. The configuration system supports 150 different options covering everything from exfiltration timing windows and file size filters to AMSI bypass, ETW bypass, and anti-dump protections. Communication blackout periods can be configured to blend with normal business hours — exfiltration defaults to 8AM–8PM to avoid standing out.

**The targeting scope matches FSB strategic priorities:** ministries of foreign affairs, embassies, defense departments, and defense-related organizations across Europe and Central Asia. Ukraine is also explicitly in scope, with Secret Blizzard known to use endpoints already compromised by Aqua Blizzard as a stepping stone.

---

## Key Technical Indicators:
- **Malware family:** Kazuar — .NET based, active since 2017, code lineage to 2005
- **Module types:** Kernel (coordinator), Bridge (external C2 proxy), Worker (collection)
- **Delivery:** Pelmeni dropper with encrypted embedded payload — sometimes hostname-bound; ShadowLoader used as alternate
- **Leader election:** single elected Kernel leader handles all external C2 — all other nodes stay silent
- **IPC mechanisms:** Windows Messaging (default), Mailslots, Named Pipes — AES encrypted, serialized via Protobuf
- **External C2 protocols:** HTTP (default), WebSockets, Exchange Web Services (EWS)
- **Configuration:** 150 options — covers exfiltration timing, security bypasses, task management, file harvesting, surveillance
- **Security bypasses:** AMSI bypass, ETW bypass, WLDP bypass, anti-dump protections
- **Collection scope:** keystrokes, screenshots, email content, browser data, running processes, USB devices, network info, user accounts, installed software
- **Exfiltration window:** 8AM–8PM default — configurable blackout periods
- **SHA-256 (Kernel):** c1f278f88275e07cc03bd390fe1cbeedd55933110c6fd16de4187f4c4aaf42b9
- **SHA-256 (Bridge):** 6eb31006ca318a21eb619d008226f08e287f753aec9042269203290462eaa00d
- **SHA-256 (Worker):** 436cfce71290c2fc2f2c362541db68ced6847c66a73b55487e5e5c73b0636c85
- **SHA-256 (Loader):** 69908f05b436bd97baae56296bf9b9e734486516f9bb9938c2b8752e152315d4

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1195 — Supply Chain Compromise: Pelmeni dropper used to deliver encrypted Kazuar payload, sometimes bound to target hostname
- **TA0002 — Execution**
  - T1106 — Native API: modules communicate via Windows Messaging and Mailslots using native Windows IPC mechanisms
- **TA0003 — Persistence**
  - T1547 — Boot or Logon Autostart Execution: Kazuar maintains persistent access across reboots through working directory state and leader re-election on restart
- **TA0005 — Defense Evasion**
  - T1562.001 — Impair Defenses: Disable or Modify Tools: AMSI bypass, ETW bypass, WLDP bypass, and anti-dump protections configurable per deployment
  - T1027 — Obfuscated Files or Information: payload encrypted inside Pelmeni dropper; sometimes bound to target hostname to resist analysis
  - T1070 — Indicator Removal: leader election keeps all non-leader nodes silent — minimizes observable network footprint across compromised environment
- **TA0006 — Credential Access**
  - T1056.001 — Input Capture: Keylogging: Worker module runs dedicated keylogging thread with configurable buffer size and flush intervals
- **TA0009 — Collection**
  - T1113 — Screen Capture: Worker module captures screenshots automatically based on configuration or on-demand via task
  - T1114 — Email Collection: Worker collects MAPI email data from compromised hosts
  - T1005 — Data from Local System: Worker harvests files, recent documents, browser data, running processes, USB devices, and full system info
- **TA0011 — Command and Control**
  - T1071.001 — Application Layer Protocol: Web Protocols: HTTP and WebSocket used for external C2 communication via Bridge module
  - T1071.003 — Application Layer Protocol: Mail Protocols: Exchange Web Services used as alternate C2 channel
  - T1090 — Proxy: Bridge module proxies all external C2 traffic on behalf of the elected Kernel leader
- **TA0010 — Exfiltration**
  - T1029 — Scheduled Transfer: exfiltration configured to occur within defined time windows — default 8AM to 8PM — with chunk size and rate limiting controls

---

## Strategic Context

What makes this worth documenting is not that Kazuar exists — it has been known for years. What matters is the architectural direction Secret Blizzard chose. Most Russian threat actors are moving toward living-off-the-land techniques, using built-in Windows tools to blend in. Secret Blizzard went the opposite way and built stealth directly into the malware itself.

The leader election design is the clearest example of that thinking. A botnet where only one machine generates outbound traffic at any given time is not something traditional network monitoring is set up to catch. If you are only looking for anomalous traffic patterns, you will miss this completely. The rest of the infected machines are just sitting quietly, collecting.

The 150-configuration option system is also significant. This is not a tool built for a single campaign. It is a platform designed for long-term, flexible operations across many different target environments. The ability to set exfiltration blackout periods, bind payloads to specific hostnames, and swap C2 protocols on the fly suggests an actor that expects to be inside target networks for months or years, not days.

The Ukraine angle connects this to the broader Russo-Ukrainian conflict cyber dimension. Secret Blizzard is documented to use endpoints already compromised by Aqua Blizzard as an entry point — one Russian APT piggybacking on another's access. That level of coordination between FSB-linked groups is a recurring pattern worth tracking.

---

## Russian Language Context

Secret Blizzard operates under the FSB's Федеральная служба безопасности (Federal'naya sluzhba bezopasnosti) — Federal Security Service — specifically Centre 16, responsible for electronic intelligence and technical penetration of foreign targets. Kazuar's architecture is designed around скрытность (skrytnost') — stealth — with the leader election mechanism ensuring minimal внешний трафик (veshniy trafik) — external traffic — across compromised networks. The targeting of дипломатические организации (diplomaticheskiye organizatsii) — diplomatic organizations — and министерства иностранных дел (ministerstva inostrannykh del) — ministries of foreign affairs — across Europe and Central Asia reflects long-standing FSB intelligence collection priorities tied to Russian внешняя политика (vneshnyaya politika) — foreign policy objectives.

---

# Incident #005 — May 20, 2026

**Target:** Government agencies, ministries of foreign affairs, law enforcement, email and cloud service providers across North Africa, Central America, Southeast Asia, and Europe

**Sector:** Government / Diplomatic / Critical Infrastructure

**Threat Actor:** APT28 / Storm-2754 (aka Forest Blizzard, Fancy Bear, Sednit, Sofacy)

**Origin:** Russia — GRU Military Unit 26165, 85th Main Special Service Centre (GTsSS) — state-sponsored espionage

**Source:** The Hacker News — April 7, 2026 | Microsoft Threat Intelligence 

**Attack Type:** SOHO Router Compromise — DNS Hijacking — Adversary-in-the-Middle — Credential Theft

**Labels:** APT28 | Forest Blizzard | GRU | FrostArmada | Operation Masquerade | DNS Hijacking | AitM | TP-Link | MikroTik | CVE-2023-50224 | SOHO Routers | Credential Theft | Disrupted

---

## Analysis

Since at least May 2025, APT28 has been running a large-scale DNS hijacking campaign targeting SOHO routers — primarily TP-Link and MikroTik devices — across more than 120 countries. Lumen's Black Lotus Labs named the campaign FrostArmada. The US DoJ coordinated a court-authorized takedown, Operation Masquerade, to disrupt the US portion of the network in April 2026.

The attack chain is straightforward but effective. APT28 gains remote admin access to vulnerable SOHO routers and rewrites their DHCP/DNS settings to point to actor-controlled DNS resolvers. Every device connected to that router — laptops, phones, anything — inherits the new settings. From that point, APT28 has visibility into all DNS lookups on the network. For targets of intelligence value, the actor serves fraudulent DNS records, redirecting traffic to AitM nodes that intercept and harvest credentials before forwarding the connection normally.

The entry vector for TP-Link WR841N routers was CVE-2023-50224 (CVSS 6.5), an authentication bypass that allows credential extraction via crafted HTTP GET requests. At peak activity in December 2025, over 18,000 unique IPs across 120+ countries were communicating with APT28 infrastructure. Microsoft identified more than 200 organizations and 5,000 consumer devices compromised. Targets included ministries of foreign affairs, law enforcement agencies, and cloud and email providers across North Africa, Central America, Southeast Asia, and Europe. A second cluster of servers was identified conducting interactive operations against a small number of MikroTik routers in Ukraine.

The approach is described as opportunistic first, then filtered. APT28 casts a wide net across vulnerable routers, gains passive DNS visibility over a large pool of users, then filters down to targets of actual intelligence value before conducting active AitM interception. This keeps the noise low and the targeting precise.

---

## Key Technical Indicators:
- **Campaign name:** FrostArmada (Lumen Black Lotus Labs) | Takedown: Operation Masquerade (FBI/DoJ)
- **Active since:** May 2025 — widespread exploitation from August 2025 — peak December 2025 (18,000+ IPs, 120+ countries)
- **Entry vector:** CVE-2023-50224 (CVSS 6.5) — TP-Link WR841N authentication bypass via crafted HTTP GET requests
- **Target devices:** TP-Link WR841N and MikroTik SOHO routers
- **Attack method:** DHCP/DNS settings overwritten to point to actor-controlled resolvers
- **AitM targets:** Microsoft Outlook Web Access and non-Microsoft government servers (at least 3 in Africa confirmed)
- **Credentials harvested:** passwords, OAuth tokens, web and email service credentials
- **Scale:** 200+ organizations, 5,000+ consumer devices (Microsoft assessment)
- **Ukraine cluster:** second server cluster conducting interactive operations against MikroTik routers in Ukraine
- **Attribution:** GRU Military Unit 26165 — confirmed by DoJ, NCSC-UK, Microsoft, Lumen

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: CVE-2023-50224 exploited to gain remote administrative access to TP-Link WR841N routers
- **TA0005 — Defense Evasion**
  - T1599.001 — Network Boundary Bridging: Router firmware and settings modified at the network edge — upstream of enterprise targets and less closely monitored than endpoint devices
- **TA0006 — Credential Access**
  - T1557 — Adversary-in-the-Middle: DNS redirected to actor-controlled resolvers — fraudulent DNS records served for specific domains to intercept TLS-encrypted traffic and harvest credentials
  - T1040 — Network Sniffing: actor-controlled DNS infrastructure provides passive visibility into all DNS lookups across compromised networks before active AitM targeting
- **TA0009 — Collection**
  - T1114 — Email Collection: Microsoft Outlook Web Access specifically targeted via fraudulent DNS records — credentials for email access harvested via AitM interception
- **TA0011 — Command and Control**
  - T1583.002 — Acquire Infrastructure: Virtual Private Servers: actor-configured VPS infrastructure operated as malicious DNS resolvers receiving requests from compromised routers

---

## Strategic Context

This campaign is a good example of how GRU operations work at scale. Rather than going after high-value targets directly and risking detection, APT28 compromised thousands of cheap, poorly maintained home and small office routers and turned them into a passive global surveillance network. The routers are upstream of larger targets, less monitored, and often running outdated firmware with no patching cycle.

The filtering approach is operationally smart. Gaining DNS visibility over 18,000 IPs and then triaging for intelligence value is much quieter than directly probing government networks. By the time active AitM interception begins against a specific target, the initial access vector is already invisible in the noise.

The Ukraine cluster is consistent with APT28's ongoing tasking in the context of the war. A separate server cluster conducting interactive operations against Ukrainian MikroTik routers is exactly the kind of persistent tactical intelligence collection the GRU needs for military operations.

---

## Russian Language Context

APT28 operates under the ГРУ (GRU) — Главное разведывательное управление (Glavnoye razvedyvatel'noye upravleniye) — Main Intelligence Directorate of the General Staff, specifically Войсковая часть 26165 (Voyskavaya chast' 26165) — Military Unit 26165. The campaign's core technique, перехват DNS (perekhvat DNS) — DNS interception — enabled пассивный сбор данных (passivnyy sbor dannykh) — passive data collection — across compromised networks. Targeting of министерства иностранных дел (ministerstva inostrannykh del) — ministries of foreign affairs — is consistent with GRU's long-standing focus on дипломатическая разведка (diplomaticheskaya razvedka) — diplomatic intelligence collection.

---

# ⚠️ Cross-Sector Alert: Webworm — Chinese Espionage Targeting Russian Government and Aerospace
**Status:** Active — EchoCreep and GraphWorm backdoors deployed, activity ongoing as of May 2026  
**Target:** Government agencies, IT services, and aerospace sector organizations in Russia, Georgia, Mongolia, and Central Asia — European government entities in Belgium, Italy, Serbia, Poland, and Spain also in scope  
**Note:** Webworm is a China-aligned actor. Russian organizations are a core target, not incidental. Actor overlaps with Space Pirates, which specifically targets Russian IT firms and aerospace. Initial access vector remains unknown as of report date.

👉 **[Read Full Entry in Global-Watch](../Global-Watch/MAY-2026-LOG.md#incident-021)**

---

# ⚠️ Cross-Sector Alert: Calypso (Red Lamassu) — China-Aligned Actor with Documented Russian Targeting
**Status:** Active — Showboat Linux backdoor deployed since mid-2022, ongoing as of May 2026   
**Target:** Telecommunications and state institutions — documented targeting includes Russia alongside Brazil, India, Kazakhstan, Thailand, and Turkey. Secondary C2 cluster linked to possible compromise in Ukraine.   
**Note:** Russian targeting is historical and not the primary focus of this campaign. Main victims are Middle East telecom and Afghanistan ISP. Ukraine secondary cluster victim unconfirmed.

👉 **[Read Full Entry in Global-Watch](../Global-Watch/MAY-2026-LOG.md#incident-022)**

---

# Incident #006 — May 26, 2026

**Target:** Russian companies across manufacturing, logistics, retail, construction, and IT sectors

**Sector:** Private Sector / Enterprise / Cross-Industry

**Threat Actor:** Bearlyfy (aka Labubu) — pro-Ukrainian hacktivist/cybercriminal group

**Origin:** Ukraine-aligned — dual financial and sabotage motivation, active since January 2025

**Source:** The Hacker News — March 27, 2026 | F6 (Russian cybersecurity vendor)

**Attack Type:** Ransomware — Extortion + Sabotage — GenieLocker Custom Encryptor

**Labels:** Bearlyfy | Labubu | GenieLocker | PolyVice | LockBit 3 | Babuk | MeshAgent | PhantomCore | Head Mare | Ukraine-aligned | Ransomware | Russia | Sabotage

---

## Analysis

Bearlyfy is a pro-Ukrainian group that has been targeting Russian companies since January 2025, with over 70 confirmed attacks as of late March 2026. F6 characterized them as dual-purpose: financial extortion and deliberate sabotage of Russian business operations. About one in five victims pays the ransom, with demands reaching hundreds of thousands of dollars.

The group started with LockBit 3 (Black) and Babuk encryptors, then added a modified PolyVice variant from May 2025, and in March 2026 deployed their **first proprietary ransomware:** GenieLocker. Its encryption scheme is modeled on Venus/Trinity ransomware families. Unlike typical ransomware, Bearlyfy does not auto-generate ransom notes — operators craft them manually per victim, sometimes adding psychological pressure tactics.

Initial access comes through exploitation of external services and vulnerable applications. MeshAgent is dropped post-access to facilitate remote control before encryption or destruction begins. F6 identified infrastructure overlaps with PhantomCore (another Ukraine-aligned group) and confirmed collaboration with Head Mare. Within a single year, the group evolved from unsophisticated experiments to targeting major Russian enterprises.

---

## Key Technical Indicators:
- **Active since:** January 2025 — 70+ attacks confirmed as of March 2026
- **Ransomware timeline:** LockBit 3/Babuk (Jan–May 2025) → PolyVice variant (May 2025) → GenieLocker (March 2026)
- **GenieLocker:** custom Windows ransomware — encryption scheme based on Venus/Trinity families
- **Remote access tool:** MeshAgent — dropped post-exploitation for persistent remote access before encryption
- **Ransom notes:** manually crafted by operators — not auto-generated
- **Initial ransom demands:** up to €80,000 early activity — escalated to hundreds of thousands
- **Payment rate:** approximately 1 in 5 victims
- **Actor overlaps:** PhantomCore (infrastructure), Head Mare (collaboration)
- **Attribution:** F6 — high confidence

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1190 — Exploit Public-Facing Application: initial access obtained through exploitation of external-facing services and vulnerable applications
- **TA0002 — Execution**
  - T1569.002 — System Services: Service Execution: MeshAgent deployed as a remote access service following initial compromise
- **TA0003 — Persistence**
  - T1219 — Remote Access Software: MeshAgent provides persistent operator-controlled remote access prior to encryption or data destruction
- **TA0040 — Impact**
  - T1486 — Data Encrypted for Impact: GenieLocker encrypts Windows endpoints — encryption scheme based on Venus/Trinity ransomware families
  - T1485 — Data Destruction: sabotage objective documented alongside financial extortion — F6 confirmed deliberate data modification and destruction in some intrusions

---

## Strategic Context

Bearlyfy is one of the clearest examples of hacktivist-to-criminal convergence in the Russia-Ukraine conflict. They started as a sabotage group with ransomware as a side effect and evolved into a financially motivated operation that still carries out deliberate destruction. Building a custom encryptor after a year of activity is a significant investment — it signals the group intends to persist long-term.

The collaboration with Head Mare and infrastructure overlap with PhantomCore suggests loose coordination between Ukraine-aligned groups, not just independent parallel operations. Russian businesses across multiple sectors are the consistent target — this is not precision targeting, it is broad economic attrition.

---

## Russian Language Context

Bearlyfy specifically targets российский бизнес (rossiyskiy biznes) — Russian business — with the dual objective of вымогательство (vymogatelstvo) — extortion — and саботаж (sabotazh) — sabotage. The deployment of шифровальщик (shifrovalshhik) — ransomware/encryptor — against Russian коммерческие предприятия (kommercheskie predpriyatiya) — commercial enterprises — represents a direct continuation of кибервойна (kibervoyna) — cyberwarfare — as an extension of the physical conflict.

---

# Incident #007 — May 26, 2026

**Target:** Ukrainian government entities, border services, and public sector organizations

**Sector:** Government / Public Sector / Border Security

**Threat Actor:** APT28 (aka UAC-0001, Fancy Bear, Forest Blizzard) — GRU Military Unit 26165

**Origin:** Russia — GRU-linked, moderate confidence attribution by ClearSky

**Source:** The Hacker News — March 5, 2026 | ClearSky Cyber Security

**Attack Type:** Spear-Phishing → HTA Loader → BadPaw → MeowMeow Backdoor

**Labels:** APT28 | UAC-0001 | BadPaw | MeowMeow | Ukraine | Spear-Phishing | HTA | VBScript | Steganography | Sandbox Evasion | PowerShell | Russian Source Code | ClearSky

---

## Analysis

ClearSky published research on March 5, 2026 documenting a new APT28-linked campaign deploying two previously undocumented malware families against Ukrainian entities. The entry point is a spear-phishing email sent from a ukr[.]net address — a deliberate choice to establish credibility with Ukrainian targets. The email contains a link that first loads a tracking pixel to confirm the link was clicked, then redirects to download a ZIP archive.

The ZIP contains a RAR inside which is a password-protected 7-Zip file containing an executable with a double extension masquerading as a PDF. Once run, an HTA file drops a decoy document — a fake Ukrainian government receipt for a border crossing appeal — while executing BadPaw in the background. The HTA checks Windows Registry install date to abort on systems installed fewer than ten days prior, a sandbox evasion check. BadPaw extracts from a PNG image using steganography and contacts C2 to fetch MeowMeow. Both tools have anti-analysis decoy GUI behavior — clicking through the decoy interface shows a cat image and a "Meow Meow Meow" message without triggering the actual malicious code.

MeowMeow only activates with the `-v` parameter supplied by the infection chain, and only after confirming no forensic tools are running. It executes PowerShell commands remotely and supports file system read, write, and delete operations. ClearSky found Russian language strings in the source code — either an OPSEC failure or development artifacts left behind.

---

## Key Technical Indicators:
- **Delivery:** spear-phishing from ukr[.]net — link to ZIP containing RAR → password-protected 7-Zip → PDF-masquerading executable
- **Tracking:** redirect URL loads a pixel image to confirm link click before delivering payload
- **Lure:** decoy document — Ukrainian government receipt for border crossing appeal
- **Sandbox evasion:** HTA queries Windows Registry install date — aborts if OS installed fewer than 10 days ago
- **BadPaw:** .NET loader — extracts from steganographic PNG image — contacts C2 to download MeowMeow
- **BadPaw decoy:** standalone execution shows cat image GUI — "MeowMeow" button outputs "Meow Meow Meow"
- **MeowMeow activation:** requires `-v` parameter from infection chain — checks for forensic tools before executing
- **MeowMeow capabilities:** remote PowerShell execution, file read/write/delete
- **Russian language strings:** found in MeowMeow source code — OPSEC failure or development artifact
- **Attribution:** ClearSky — moderate confidence APT28 based on targeting, lure geopolitics, and TTP overlaps
- **Persistence:** scheduled task created to execute VBScript for reboot persistence

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.002 — Phishing: Spear-phishing Link: email sent from ukr[.]net delivers a link redirecting through a tracking pixel to a ZIP payload download
- **TA0002 — Execution**
  - T1059.001 — Command and Scripting Interpreter: PowerShell: MeowMeow executes remote PowerShell commands on compromised hosts
  - T1059.005 — Command and Scripting Interpreter: Visual Basic: VBScript extracted by HTA handles BadPaw extraction and scheduled persistence
- **TA0003 — Persistence**
  - T1053.005 — Scheduled Task/Job: Scheduled Task: scheduled task created to execute VBScript on system reboot
- **TA0005 — Defense Evasion**
  - T1027.003 — Obfuscated Files or Information: Steganography: BadPaw loader embedded within a PNG image and extracted at runtime
  - T1036.007 — Masquerading: Double File Extension: executable uses .pdf.exe double extension to appear as a document
  - T1497.001 — Virtualization/Sandbox Evasion: System Checks: HTA queries OS install date — aborts if system is fewer than 10 days old; MeowMeow checks for active forensic tools before executing
- **TA0007 — Discovery**
  - T1518.001 — Software Discovery: Security Software Discovery: MeowMeow checks for Wireshark, Procmon, OllyDbg, and Fiddler before activating malicious code

---

## Strategic Context

The ukr[.]net sender address and Ukrainian border crossing lure are deliberate social engineering choices — APT28 is specifically tailoring this campaign to Ukrainian government staff who would routinely handle border crossing documentation. The multi-layer archive chain (ZIP → RAR → 7-Zip → executable) is designed to exhaust automated sandboxes and delay detonation.

The Russian strings in MeowMeow's source code are worth noting. ClearSky raised two possibilities: an OPSEC error where the developer forgot to strip Russian artifacts, or an intentional leave-behind. Either way it reinforces the Russian origin assessment.

---

## Russian Language Context

APT28's use of украинский пограничный контроль (ukrainskiy pogranichnyy kontrol') — Ukrainian border control — as a lure theme targets сотрудники правительства (sotrudniki pravitelstva) — government employees — handling официальные документы (ofitsialnye dokumenty) — official documents. The discovery of строки на русском языке (stroki na russkom yazyke) — Russian language strings — in the malware source code directly implicates русскоязычный разработчик (russikoyazychnyy razrabotchik) — a Russian-speaking developer.

---

# Incident #008 — May 26, 2026

**Target:** Unnamed European financial institution — senior legal and policy advisor involved in regional development and reconstruction procurement

**Sector:** Financial Services / European Policy / Reconstruction Finance

**Threat Actor:** UAC-0050 (aka DaVinci Group, Mercenary Akula) — Russia-aligned mercenary group

**Origin:** Russia — assessed affiliated with Russian law enforcement agencies (CERT-UA) — espionage and financial theft

**Source:** The Hacker News — February 24, 2026 | BlueVoyant (Patrick McHale, Joshua Green)

**Attack Type:** Spear-Phishing → Multi-Layer Archive → RMS Remote Access Deployment

**Labels:** UAC-0050 | DaVinci Group | Mercenary Akula | RMS | Remote Manipulator System | Spear-Phishing | PixelDrain | Spoofed Domain | Double Extension | Ukraine | Europe | Financial | Espionage | Fire Cells

---

## Analysis

BlueVoyant published research on February 24, 2026 documenting a UAC-0050 attack against an unnamed European financial institution involved in Ukraine-related regional development and reconstruction work. The specific target was a senior legal and policy advisor with privileged access to procurement operations. CERT-UA has previously characterized UAC-0050 as a mercenary group affiliated with Russian law enforcement operating under the Fire Cells branding.

The attack began with a spear-phishing email spoofing a Ukrainian judicial domain, using legal themes to direct the target to download an archive hosted on PixelDrain — a file-sharing service chosen to bypass reputation-based security filters. The archive chain is **deliberately layered:** ZIP containing a RAR containing a password-protected 7-Zip file containing an executable disguised as a PDF via double extension. Execution deploys an MSI installer for Remote Manipulator System, a legitimate Russian remote desktop software that provides full remote control, desktop sharing, and file transfer.

BlueVoyant noted this attack represents a notable shift for UAC-0050, which has historically focused almost exclusively on Ukrainian targets — particularly accountants and financial officers. Targeting a European institution involved in Ukraine reconstruction signals possible probing of Ukraine-supporting Western entities, consistent with CrowdStrike's 2026 Global Threat Report assessment of Russia-nexus actors expanding toward NATO-adjacent targets.

---

## Key Technical Indicators:
- **Sender:** spoofed Ukrainian judicial domain — legal-themed lure
- **Hosting:** payload hosted on PixelDrain to avoid reputation-based blocks
- **Archive chain:** ZIP → RAR → password-protected 7-Zip → PDF-masquerading executable (double extension)
- **Payload:** MSI installer deploying Remote Manipulator System (RMS) — Russian remote desktop software
- **RMS capabilities:** full remote control, desktop sharing, file transfer
- **Target profile:** senior legal/policy advisor with procurement access — Ukraine reconstruction finance
- **Prior modus operandi:** LiteManager, RemcosRAT against Ukrainian accountants and financial officers
- **CERT-UA characterization:** mercenary group affiliated with Russian law enforcement — Fire Cells branding
- **BlueVoyant designation:** Mercenary Akula
- **Shift significance:** first documented European financial institution targeting by this group

---

## MITRE ATT&CK Tactics and Techniques:
- **TA0001 — Initial Access**
  - T1566.002 — Phishing: Spear-phishing Link: legal-themed email spoofing Ukrainian judicial domain — link directs target to PixelDrain-hosted archive payload
- **TA0002 — Execution**
  - T1204.002 — User Execution: Malicious File: target executes PDF-masquerading executable triggering MSI installer deployment
- **TA0005 — Defense Evasion**
  - T1036.007 — Masquerading: Double File Extension: executable uses .pdf.exe double extension to appear as a document
  - T1583.006 — Acquire Infrastructure: Web Services: PixelDrain used as payload hosting to bypass reputation-based security controls
- **TA0011 — Command and Control**
  - T1219 — Remote Access Software: Remote Manipulator System deployed as legitimate remote desktop tool — provides persistent stealthy access evading traditional AV detection

---

## Strategic Context

UAC-0050 choosing a European reconstruction finance institution as a target makes sense from a Russian intelligence standpoint. Ukraine-related reconstruction funding flows, procurement decisions, and institutional relationships are all strategically valuable. The senior legal/policy advisor profile is a deliberate pick — someone with broad visibility into institutional operations rather than a single account.

The use of RMS is a recurring signature. It's legitimate software, it's Russian, and it's been in UAC-0050's toolkit consistently. The layered archive chain is also consistent with the group's past operations. The expansion beyond Ukraine is the story here — this group's geographic scope is widening.

---

## Russian Language Context

UAC-0050 is assessed by CERT-UA as affiliated with российские правоохранительные органы (rossiyskiye pravookhranitelnyye organy) — Russian law enforcement agencies. The use of подделка судебного домена (poddelka sudebnogo domena) — judicial domain spoofing — and легитимное программное обеспечение для удалённого доступа (legitimnoye programmnoye obespecheniye dlya udalyonnogo dostupa) — legitimate remote access software — reflects a consistent preference for правдоподобное отрицание (pravdopodobnoye otritsaniye) — plausible deniability — in UAC-0050 operations.
