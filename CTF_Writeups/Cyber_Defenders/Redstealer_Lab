# Lab Investigation: Red Stealer Lab

| Metadata | Details |
| :--- | :--- |
| **Platform** | CyberDefenders
| **Category** | Threat Intel |
| **Difficulty** | Easy |
| **Tactics** | Execution, persistence, privilege escalation, stealth, defense impairment, discovery, collection, impact |
| **Primary Tools Used** | `whois`, `virustotal`, `malwarebazaar`, `threatfox`, `ANY.RUN`|

---

## Incident Scenario & Objective

> **Scenario:** You are part of the Threat Intelligence team in the SOC (Security Operations Center). An executable file has been discovered on a colleague's computer, and it's suspected to be linked to a Command and Control (C2) server, indicating a potential malware infection. Your task is to investigate this executable by analyzing its hash. The goal is to gather and analyze data beneficial to other SOC members, including the Incident Response team, to respond to this suspicious behavior efficiently.

---

## Lab Questions & Findings

### Q1 - Categorizing malware enables a quicker and easier understanding of its distinct behaviors and attack vectors. What is the category of the identified malware?

Today, we're using VirusTotal! VirusTotal is totally new to me, so I decided to do a little bit of research before starting to answer the questions. I was instructed to enter the hash provided when downloading the lab's folder into VirusTotal, as shown in Screenshot 1. Screenshot 2 shows what I saw when I entered the hash.

Immediately, I noticed that a lot of security vendors had flagged the hash as malicious. There were three categories shown under 'Threat Categories': Trojan, Dropper, and PUA, as shown in Screenshot 3.

Entering 'Trojan' for this question was the correct answer!

### Q2 - Clearly identifying the name of the malware file improves communication among the SOC team. What is the file name associated with this malware?

After finding the category, I went to the Details tab and scrolled down a bit to the Names section, as shown in Screenshot 4. The name of the file is wextract.

### Q3 - Knowing the exact timestamp of when the malware was first observed can help prioritise response actions. Newly detected malware may require urgent containment and eradication compared to older, well-documented threats. What is the UTC timestamp of the malware's first submission to VirusTotal?

Also shown in Screenshot 4 is the UTC timestamp of when the malware was first submitted to VirusTotal: 2023-10-06 04:41.

### Q4 - Understanding the techniques used by malware helps in strategic security planning. What is the MITRE ATT&CK technique ID for the malware collecting data from the local system prior to exfiltration?

MITRE ATT&CK is a knowledge base of adversary tactics and techniques based on real-world situations. It contains lists of techniques that can be used by attackers to infiltrate and harm computers. This database helps security professionals identify attacks and understand how to mitigate them. All techniques have IDs assigned to them by MITRE ATT&CK.

Going into the 'Collection' section of the Behaviour tab, the malware seems to search for sensitive data from the web browser 'Comodo IceDragon' by file. The ID for this is T1005, as shown in Screenshot 5.

### Q5 - Following execution, the malware performs a connectivity check by resolving a well-known social-media domain before contacting its C2. Which single domain does it resolve for this check?

Staying in the Behaviour tab, I looked under the DNS Resolutions section. Since the question says the malware resolves a well-known social-media domain before contacting the C2 server, the answer should stand out among all the other DNS resolution entries.

Screenshot 6 clearly shows that the social-media domain is facebook.com.

### Q6 - Once the malicious IP addresses are identified, network security devices such as firewalls can be configured to block traffic to and from these addresses. Can you provide the IP address and destination port the malware communicates with?

The malware is configured to communicate with the IP address 77.91.124.55 on port 19071. This information was found under the Memory Pattern URLs section, as shown in Screenshot 7.

### Q7 - YARA rules are designed to identify specific malware patterns and behaviours. Using MalwareBazaar, what's the name of the YARA rule created by "Varp0s" that detects the identified malware?

YARA rules are used to identify and classify malware, suspicious files, and memory artefacts. They use signature-based pattern-matching definitions that can be used by tools and analysts. A YARA rule contains a name, metadata (such as the author and date), strings, and a condition that dictates how the strings must combine to trigger a match.

In digital forensics, they enhance the identification of malware within large systems or datasets.

Using MalwareBazaar helps because it is an online platform that security analysts and others in the field use to share information about threats and malware. The rule name created by Varp0s is detect_Redline_Stealer, which relates to the malware being investigated. See Screenshot 8 for evidence.

### Q8 - Understanding which malware families are targeting the organisation helps in strategic security planning for the future and prioritising resources based on the threat. Can you provide the different malware alias associated with the malicious IP address according to Malpedia?

Malpedia is a free intelligence resource designed to provide information that helps identify malware and offers actionable context when investigating it. Malpedia is also operated as an invite-only trust group, meaning that access is restricted to trusted individuals with an account.

Searching for 'RedLine Stealer' in its database brings up the alias directly underneath: RECORDSTEALER. See Screenshot 9 for evidence.

### Q9 - By identifying the malware's imported DLLs, we can configure security tools to monitor for the loading or unusual usage of these specific DLLs. Can you provide the DLL utilised by the malware for privilege escalation?

A Dynamic-Link Library (DLL) was identified in connection with privilege escalation. This refers to an attempt by malware to obtain higher permissions than those initially granted, potentially moving from a regular user level to administrative access.

Going back to the Details tab, there is a DLL called ADVAPI32.dll, as shown in Screenshot 10.

This DLL contains a number of functions, such as AdjustTokenPrivileges. These functions allow applications to interact with security tokens and privileges, which help determine what actions a user or process can perform. Malware may attempt to abuse token privileges to gain unauthorised access to protected resources.

## Verdict and Incident Recommendations

### Verdict

The investigation identified the sample as a malicious stealer-type Trojan associated with RedLine Stealer. Analysis using VirusTotal revealed malicious network activity and command-and-control (C2) infrastructure. The malware performs DNS lookups, communicates with a remote server, and is associated with collecting information from the infected system before potential exfiltration. The findings indicate a risk of sensitive information theft and unauthorised access to user data. The identified indicators of compromise (IOCs), including the malware's hash and C2 network indicators, should be shared with the Incident Response team to support containment and further investigation.

### Remediation Steps

Network security devices, such as firewalls, should be configured to block malicious traffic and mitigate any threats posed to the computer system. Adding verified malicious C2 IP addresses and relevant domains to firewall blocklists will help prevent communication with this particular malware infrastructure. Running antivirus scans, removing malicious files, resetting affected passwords, and revoking active session tokens are also necessary to protect the computer and its accounts.

## References + achievement link (if applicable)

https://cyberdefenders.org/blueteam-ctf-challenges/red-stealer/ 
https://www.veeam.com/blog/yara-rules-malware-detection-analysis.html 
https://malpedia.caad.fkie.fraunhofer.de/details/win.redline_stealer 
