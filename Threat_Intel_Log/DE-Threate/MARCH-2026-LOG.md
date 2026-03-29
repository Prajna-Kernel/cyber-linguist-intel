**Incident #001 — March 28, 2026**

**Target:** Citrix NetScaler ADC and NetScaler Gateway (Global Enterprise Infrastructure)

**Sector:** Technology / Network Infrastructure / Critical Enterprise Access

**Threat Actor:** Unknown — Active Reconnaissance Observed

**Origin:** Unattributed

**Source:** The Hacker News — March 28, 2026 | Defused Cyber | watchTowr

**Attack Type:** Active Reconnaissance — Pre-Exploitation Phase (CVE-2026-3055)

**Labels:** CVE-2026-3055 | CVSS 9.3 | Memory Overread | SAML IDP | Network Security

---

**Analysis**
CVE-2026-3055 is a critical vulnerability in Citrix NetScaler ADC and NetScaler Gateway caused by insufficient input validation, resulting in a memory overread condition. An attacker exploiting this flaw can leak sensitive information from memory — including authentication tokens, session data, or configuration details — without requiring prior authentication, provided the appliance is configured as a SAML Identity Provider (SAML IDP).

As of March 28, 2026, the vulnerability has not yet been actively exploited — but active reconnaissance is confirmed.

---

**Key Technical Indicators:**
- **Vulnerability Type:** Memory Overread — Insufficient Input Validation

- **Attack Vector:** SAML IDP configuration endpoint

- **Recon Method:** Attackers probing /cgi/GetAuthMethods to fingerprint authentication flows and identify SAML IDP-configured instances

- **Affected Versions:** NetScaler ADC/Gateway 14.1 before 14.1-66.59 | 13.1 before 13.1-62.23 | 13.1-FIPS and NDcPP before 13.1-37.262

- **Detection:** Confirmed in honeypot networks by Defused Cyber and watchTowr

---

**Strategic Context**
Citrix NetScaler is a widely deployed enterprise remote access solution — a high-value target because compromising it provides an authenticated foothold into corporate and government networks. This CVE follows a pattern of NetScaler vulnerabilities being rapidly weaponised after disclosure, including CVE-2023-4966 (Citrix Bleed) and CVE-2025-5777 (Citrix Bleed 2).

The shift from recon to active exploitation is expected imminently. Organisations running affected versions have a rapidly closing window to patch.
