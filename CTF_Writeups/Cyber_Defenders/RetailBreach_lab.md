# Lab Investigation: RetailBranch Lab

| Metadata | Details |
| :--- | :--- |
| **Platform** | CyberDefenders
| **Category** | Network Forensics |
| **Difficulty** | Easy |
| **Tactics** | Reconnaissance, Initial Access, Execution, Credential Access, Discovery, Lateral Movement |
| **Primary Tools Used** | `Wireshark`, `Network Miner`, `Brim`|

---

## Incident Scenario & Objective

> **Scenario:** In recent days, ShopSphere, a prominent online retail platform, has experienced unusual administrative login activity during late-night hours. These logins coincide with an influx of customer complaints about unexplained account anomalies, raising concerns about a potential security breach. Initial observations suggest unauthorized access to administrative accounts, potentially indicating deeper system compromise. Your mission is to investigate the captured network traffic to determine the nature and source of the breach. Identifying how the attackers infiltrated the system and pinpointing their methods will be critical to understanding the attack's scope and mitigating its impact.

---

## Lab Questions & Findings

### Q1 - Identifying an attacker's IP address is crucial for mapping the attack's extent and planning an effective response. What is the attacker's IP address?

Alright, so after opening *RetailBreach.pcap* file on Wireshark (I'm more comfortable using Wireshark considering it's primarily what I use at university), I opened endpoints from the statistics menu.

(see screenshot 1 in the *screenshots_retailbreach* folder.)

clicking on IPv4, there are some IP addresses. We need to determine which of the three is the attacker's IP - and when looking at the incoming requests, we can see which one it is fairly easily. 

(see screenshot 2 in the *screenshots_retailbreach* folder.)

### Q2 - The attacker used a directory brute-forcing tool to discover hidden paths. Which tool did the attacker use to perform the brute-forcing?

### Q3 - Cross-Site Scripting (XSS) allows attackers to inject malicious scripts into web pages viewed by users. Can you specify the XSS payload that the attacker used to compromise the integrity of the web application?

### Q4 - Pinpointing the exact moment an admin user encounters the injected malicious script is crucial for understanding the timeline of a security breach. Can you provide the UTC timestamp when the admin user first visited the page containing the injected malicious script?

### Q5 - The theft of a session token through XSS is a serious security breach that allows unauthorized access. Can you provide the session token that the attacker acquired and used for this unauthorized access?

### Q6 - Identifying which scripts have been exploited is crucial for mitigating vulnerabilities in a web application. What is the name of the script that was exploited by the attacker?

### Q7 - Exploiting vulnerabilities to access sensitive system files is a common tactic used by attackers. Can you identify the specific payload the attacker used to access a sensitive system file?

## Verdict and incident recommendations 

### Verdict

### Remediation Steps