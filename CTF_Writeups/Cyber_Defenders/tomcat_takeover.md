# Lab Investigation: tomcat takeover

| Metadata | Details |
| :--- | :--- |
| **Platform** | CyberDefender |
| **Category** | Network Analysis|
| **Difficulty** | Easy |
| **Primary Tools Used** | `Wireshark`, `Network Miner` |

---

## Incident Scenario & Objective

> **Scenario:** The SOC team has identified suspicious activity on a web server within the company's intranet. To better understand the situation, they have captured network traffic for analysis. The PCAP file may contain evidence of malicious activities that led to the compromise of the Apache Tomcat web server. Your task is to analyze the PCAP file to understand the scope of the attack.

---

## Lab Questions & Findings

### Q1 - Given the suspicious activity detected on the web server, the PCAP file reveals a series of requests across various ports, indicating potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?

In screenshot one, I went to the statistics at the top of wireshark and then into IPv4 statistics. In the drop down I then selected source and destination addresses, which shows all of the potential answers to this question as shown in screenshot 2.

### Q2 - Based on the identified IP address associated with the attacker, can you identify the country from which the attacker's activities originated?

### Q3 - From the PCAP file, multiple open ports were detected as a result of the attacker's active scan. Which of these ports provides access to the web server admin panel?

### Q4 - Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?

### Q5 - After enumerating directories on our web server, the attacker made numerous requests to identify administrative interfaces. Which directory related to the admin panel did the attacker uncover? (Provide the path including the leading slash, e.g. /path)

### Q6 - After accessing the admin panel, the attacker brute-forced the login. What credentials did the attacker successfully use? (Provide them in username:password format)

### Q7 - Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?

### Q8 - After the attacker established a reverse shell on our server, the payload connects back to the attacker's machine. From the analysis, what is the callback destination in IP:port format?

## Verdict and incident recommendations 

### Verdict

### Remediation Steps

## References + achievement link (if applicable)

https://whatismyipaddress.com/ip-lookup
https://ipfinder.app/en/learn/ip-address/how-ip-address-lookup-works/ 
https://www.researchgate.net/publication/382051703_How_dangerous_to_be_open_Ports_80 
