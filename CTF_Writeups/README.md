# Cyber Threat Framework & CTF Write-Ups

A structured collection of investigation reports, lab walkthroughs, and challenge write-ups across various Digital Forensics, Incident Response (DFIR), and Web Security platforms.

---

## Platforms & Profiles

| Platform | Profile / Status | Focus Areas |
| :--- | :--- | :--- |
| **CyberDefenders** | [https://cyberdefenders.org/p/immy1482](#) | Blue Team Labs, Disk/Memory Forensics, Network PCAPs |
| **Hack The Box** | [Your Profile Link](#) | Sherlocks (DFIR), Retired Machines, Web Security |
| **TryHackMe** | [https://tryhackme.com/p/imogenanderson25](#) | Blue Team Pathways, SOC Analyst Labs, OWASP |
| **Blue Team Labs Online** | [https://blueteamlabs.online/home/user/d964d4cc6848a065ebe8f4](#) | Incident Response Scenarios, Log Analysis |

---

## 📂 Write-Up Index

### 🔍 Digital Forensics & Incident Response (DFIR)

| ID | Lab / Challenge Name | Platform | Category / Domain | Key Tools Used | Report Link |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DF-01** | *[Insert Lab Name]* | CyberDefenders | Memory Forensics | Volatility 3, WinPmem | [View Report](./cyberdefenders/lab-01.md) |
| **DF-02** | *[Insert Sherlock Name]* | Hack The Box | Disk & Artifact Analysis | Eric Zimmerman Tools, KAPE | [View Report](./hackthebox/sherlock-01.md) |
| **DF-03** | *[Insert Lab Name]* | Blue Team Labs | Network Analysis | Wireshark, NetworkMiner | [View Report](./btlo/lab-01.md) |

### 🌐 Web Security & Fundamentals

| ID | Lab / Module Name | Platform | Vulnerability / Topic | Key Tools Used 
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WEB-01**| Detecting Web Attacks | LetsDefend | Web Fundamentals, SOC analysing | SQL Inection, Cross Site scripting, IDOR.
| **WEB-02**| *XXE Infiltration Lab Writeup* | CyberDefenders | XXE Injectors | Burp Suite Repeater | [View Report](./portswigger/lab-01.md) |

---

## 🛠️ Preferred Analysis Stack

* **Disk & Artifact Analysis:** FTK Imager, Autopsy, KAPE, Eric Zimmerman Suite (`PECmd`, `MFTECmd`, `EvtxECmd`)
* **Memory Forensics:** Volatility 2 & 3, Redline
* **Network Traffic Analysis:** Wireshark, NetworkMiner, tshark
* **Web & Utilities:** Burp Suite, CyberChef, OWASP ZAP, Python 3

---

## ⚠️ Responsible Disclosure & Anti-Spoiler Policy

* **Active Challenges:** No active Hack The Box machines, active Sherlocks, or ongoing CTF competition answers are hosted in this repository in compliance with platform policies.
* **Retired Content:** All solutions featured here pertain to retired challenges or public educational labs.
* **Sanitized Answers:** Where required by platform terms, final flags are omitted or masked to preserve the educational experience for others.