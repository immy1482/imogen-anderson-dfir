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

### Lab Questions and Findings

### Q1: What is the name of the file associated with the given hash?

IcedID can refer to different things in the world of cyber security. In this investigation, IcedID refers to a banking trojan and initial access malware, rather than an unrelated incident involving a leak of personal information belonging to U.S. Immigration and Customs Enforcement (ICE).

To investigate the sample, I used VirusTotal to search for the provided hash. Screenshot 1 shows the initial results page.

After scrolling through the file details, several filenames were listed as being associated with the sample. The filename identified for this investigation was document-1982481273.xlsm.

The .xlsm extension indicates a Microsoft Excel macro-enabled workbook, which can contain macros and other executable code. Such documents can be used in phishing campaigns to deliver malware or initiate further malicious activity. Because spreadsheets are commonly used in business environments, a malicious document may appear legitimate to an unsuspecting employee.

However, the file extension alone does not establish that the document contains malicious macros or that any code was executed.

### Q2: Can you identify the filename of the GIF file that was deployed?

I navigated to the Behaviour tab in VirusTotal and scrolled down to the list of dropped files. Only one GIF file was listed: 3003.gif, as shown in Screenshot 2.

Screenshot 3 provides additional information about the file and its associations with different URLs.

The presence of this file in the dropped-files section indicates that it was associated with the sample's observed behaviour. However, further analysis would be needed to establish precisely how the file was used and whether it contained any malicious functionality. Its presence alone does not prove that the GIF itself was executable malware.

### Q3: How many domains does the malware contact to download the additional payload identified in Q2?

The answer is five domains.

Using the contacted URLs shown in Screenshot 3, I identified which URLs referenced the 3003.gif file. Five of the listed domains were associated with URLs containing this filename, while three others did not appear to reference it.

Using multiple domains can provide malware operators with alternative locations for hosting or retrieving files. This may help maintain access to payloads if one domain becomes unavailable or is blocked.

However, the presence of multiple URLs does not necessarily mean that all five downloads succeeded or that every domain was contacted during every execution.

### Q4: From the domains identified in Q3, which registrar was predominantly used by the threat actor to host their malicious content? Specify the registrar of the domain ending in .com.

I examined the domains identified during the previous question and investigated their registration information.

One of the domains associated with the sample was tajushariya.com. As shown in Screenshot 4, its registrar was identified as NameCheap, Inc.

Domain registration information can help investigators identify infrastructure associated with suspicious activity. However, a domain's registrar is not necessarily responsible for the malicious activity conducted through that domain.

### Q5: Can you identify the threat actor associated with the provided sample?

I used Malpedia, a malware research and threat intelligence resource, to investigate the threat actors associated with IcedID. Screenshot 5 shows the relevant information.

The alias specifically required by CyberDefenders was Gold Cabin.

Gold Cabin is an alias associated with the cybercriminal activity surrounding IcedID. The group's operations involved distributing malware through phishing campaigns and malicious documents, including documents delivered in password-protected ZIP archives. These campaigns could use existing email conversations to make messages appear more credible and persuade recipients to open malicious attachments.

The U.S. Department of Justice also announced criminal proceedings involving IcedID operators, including co-manager Ilya Angelov, who was sentenced to 24 months in prison and ordered to pay a financial penalty.

These developments illustrate the criminal activity associated with IcedID and the consequences faced by individuals involved in its operation. However, an alias associated with a malware family does not, on its own, prove that a particular actor personally conducted every observed attack.

### Q6: During the execution phase, which function does the malware use to retrieve additional payloads onto the system?

The function identified was URLDownloadToFileA, as shown in Screenshot 6 from Recorded Future Triage.

URLDownloadToFileA is a Windows API function that allows an application to download a file from a URL and save it to a local file path. It can retrieve files over HTTP or HTTPS, making it useful for legitimate applications that need to download resources.

However, malware can also abuse this function to retrieve additional payloads from remote infrastructure and save them to a victim's system. This can enable a malware infection to progress beyond its initial delivery stage.

Identifying this function provides a useful behavioural indicator for investigators. When reviewing a suspicious process, analysts can examine associated network connections, downloaded files and process activity to determine whether the function was used for malicious purposes.

## Verdict and incident recommendations 

### Verdict

The investigation concluded that IceID malware was associated with a .xlsm document which was used to phish websites. During execution, the malware used  the Windows API function URLDownloadToFileA to attempt to download an additional payload named 3003.gif from five different domains. The downloaded file's .gif extension may have been used to disguise its true file type and make it appear less suspicious. It was linked to Gold Cabin, a well-known cybercriminal group. 


### Remediation Steps

1. Isolate and investigate compromised systems
2. Review the identified filenames against available security telemetry and block them on firewalls as well as the relevant domains and URLs through filtering. 
3. Strengthen email security to detect phishing messages and strange attachments
4. Investigate possible credential compromise and change passwords and account details if needed (since IceID is associated with credential theft)

## References + achievement link (if applicable)

https://malpedia.caad.fkie.fraunhofer.de/details/win.icedid 
https://malpedia.caad.fkie.fraunhofer.de/actor/gold_cabin
https://www.linkedin.com/pulse/weekly-recap-telecom-sleeper-cells-llm-jailbreaks-apple-kcx3c/ 
