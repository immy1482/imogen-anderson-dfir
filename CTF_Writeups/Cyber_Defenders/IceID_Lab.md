# Lab Investigation: IceID lab

| Metadata | Details |
| :--- | :--- |
| **Platform** | CyberDefenders |
| **Category** | Threat intel |
| **Difficulty** | Easy |
| **Primary Tools Used** | `virustotal`, `malpedia` |

---

## Incident Scenario & Objective

> **Scenario:** A cyber threat group was identified for initiating widespread phishing campaigns to distribute further malicious payloads. The most frequently encountered payloads were IcedID. You have been given a hash of an IcedID sample to analyze and monitor the activities of this advanced persistent threat (APT) group.

---

## Lab Questions & Findings

### Q1 - What is the name of the file associated with the given hash?

IceID can refer to two things in the world of cyber security - a incident where ICE (Immigration and Customs Enforcement) had all of their personal information leaked by a whistleblower, or a banking trojan and inital access malware. We'll be talking about the latter today!

Today we will be using virus total. After inputting the hash, screenshot 1 is the screen we are greeted with. 

Scrolling down on the detail pages, there are a couple names for the file associated - but the actual document has the name 'document-1982481273.xlsm'. 

Xlsm file is a excel spreadsheet format which can store and run code and custom scripts. Xlsm can be used to deliver the malware and bypass email security filters - especially for a bank which often use spreadsheets, so it's no wonder that this could be have gone past employees at banks and be flagged as a normal spreadsheet. 

### Q2 - Can you identify the filename of the GIF file that was deployed?

Going to the behaviour tab and scrolling down to the files dropped, theres only one gif that appears there - 3003.gif, as shown in screenshot 2. 

Screenshot 3 shows that the gif had been used several times for various websites, and since they were in the dropped files, they must be highly detected as malicious - designed to be executed as part of the malware's process. This is the payload.

### Q3 - How many domains does the malware look to download the additional payload file in Q2?

Well, it's 5! Since we know the answer to the last question was 3003.gif, we can look at the contacted URLs in screenshot 3 and see which ones have the 3003.gif payload and which ones don't. 5 of the domains do, 3 of them don't.

Since it attacked multiple servers / domains, the malware increases it's likelihood of successful infilitration. 

### Q4 - From the domains mentioned in Q3, a DNS registrar was predominantly used by the threat actor to host their harmful content, enabling the malware's functionality. Can you specify the Registrar INC of the domain that end with .com?

From using the names of the infected domains in Q3, we scroll down a little to contacted domains and look at the domains. One of the domains that had been used by the malware is tajushariya.com, whos registrar is NameCheap. See screenshot 4. 

### Q5 - Could you specify the threat actor linked to the sample provided?

Using Malpedia, a threat intelligence platform designed to help security analysts and other people in the cyber security industry, it gives us the names of the actors associated with IcedID, as seen in screenshot 5. 

The alias that CyberDefenders is specifically looking for is gold cabin. 

Gold cabin is a cybercriminal threat group that operates a malware distribution service for their customers. They use malicious documents in password-protected ZIP archives (which bypass basic filters) and send it through stolen email chains to download and execute payloads. 

The DOJ (U.S. Department of Justice) sentenced a co-manager, Ilya Angelov, to 24 months in prison along with a 1.7 million fine. The group has pretty much died out by then. 

### Q6 - In the Execution phase, what function does the malware employ to fetch extra payloads onto the system? 



## Verdict and incident recommendations 

### Verdict

### Remediation Steps

## References + achievement link (if applicable)

https://malpedia.caad.fkie.fraunhofer.de/actor/gold_cabin
https://www.linkedin.com/pulse/weekly-recap-telecom-sleeper-cells-llm-jailbreaks-apple-kcx3c/ 
