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
