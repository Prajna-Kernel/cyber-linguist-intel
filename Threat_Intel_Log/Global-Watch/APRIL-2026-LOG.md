# 🛡️ Threat Intelligence Log — April 2026

> Active monitoring of cross-border cyber operations and state-sponsored activity.
> Focus: Non-regional incidents, wiper attacks, and AI-assisted threat tooling.
> Language access: English

---

### Incident #007 — April 1, 2026

**Target:**  
Government Networks — Southeast Asia (Multiple Entities)  

**Sector:**  
Government / Public Administration  

**Threat Actor:**  
TrueChaos (Unattributed — Chinese-nexus, Moderate Confidence)  

**Linked actors:**  
Amaranth-Dragon (Havoc C2 usage overlap)  

**Origin:**  
China-nexus (assessed)  

**Source:**  
The Hacker News — March 31, 2026 | Check Point Research  

**Attack Type:**  
Zero-Day Exploitation — Supply Chain / Update Mechanism Abuse  

**Labels:**  
CVE-2026-3502 | CVSS 7.8 | Zero-Day | DLL Side-Loading | Supply Chain | C2  

---

**Analysis**  
TrueChaos exploited CVE-2026-3502, a zero-day in TrueConf — a Russian-developed video conferencing client widely used by governments. The flaw sits in the update mechanism: the client pulls updates from an on-premises server without checking integrity.

An attacker who compromises the server can simply swap the legitimate update with a malicious one. Once clients download it, the backdoor installs silently across the entire network. Classic supply-chain attack — one server, many victims.

**Key Technical Indicators:**  
- **Vulnerability:** Lack of integrity check in TrueConf client update mechanism  
- **Initial Access:** Attacker-controlled on-premises TrueConf server  
- **Delivery:** Poisoned update package distributed to all connected clients  
- **Execution:** Rogue installer → DLL side-loading → backdoor ("7z-x64.dll")  
- **Final Stage:** Havoc C2 framework  
- **Infrastructure:** Alibaba Cloud and Tencent (consistent with Chinese-nexus TTPs)  
- Campaign active since early 2026 — patched in version 8.5.3  

---

**Strategic Context**  
TrueConf is a Russian-developed product heavily used in government networks in Russia and several aligned countries. A Chinese-nexus actor targeting Southeast Asian governments with it shows how fluid and non-aligned state cyber operations have become.

The overlap with Amaranth-Dragon (who hit SE Asian government targets in 2025) adds moderate confidence to the attribution. This kind of update poisoning is especially dangerous because it requires zero user interaction — everything looks normal until it’s too late.

*Organisations still on TrueConf should update to version 8.5.3 immediately.*

---

**Russian Language Context**  
TrueConf is a Russian-developed secure video conferencing platform. Its update mechanism, client interface, and much of the internal documentation are in Russian. Threat actors exploiting Russian-made software often leave behind Russian-language strings, commands, or artifacts — making incidents like this excellent real-world practice for technical Russian.
