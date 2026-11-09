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

In screenshot one, I navigated to Statistics at the top of Wireshark and selected IPv4 Statistics. From the drop-down menu, I then selected Source and Destination Addresses, which displayed the different IP addresses involved in the network traffic, as shown in screenshot 2.

The correct answer was the second option under Source IPv4 Addresses, which was 14.0.0.120. This was identified as the source IP address responsible for initiating the suspicious requests against our server.

### Q2 - Based on the identified IP address associated with the attacker, can you identify the country from which the attacker's activities originated?

To identify the country associated with the attacker's IP address, I used a website called WhatIsMyIpAddress, which provides IP address geolocation information.

After entering the previously identified IP address, 14.0.0.120, the website indicated that it was associated with China, specifically the Guangdong region (see screenshot 3).

IP address lookups are useful when investigating potential security threats, as they allow security professionals to identify the approximate geographical location and network information associated with suspicious IP addresses. This can help when investigating malicious traffic, port scans and suspicious login attempts. These lookups work by using databases containing information from regional internet registries and IP geolocation providers to associate IP addresses with networks and geographical regions.

However, IP geolocation is not always completely accurate and does not necessarily identify the attacker's actual physical location.


### Q3 - From the PCAP file, multiple open ports were detected as a result of the attacker's active scan. Which of these ports provides access to the web server admin panel?

To identify which open port provided access to the web server's admin panel, I returned to Statistics and selected IPv4 Statistics, followed by Destination and Ports, as shown in screenshot 4.

I then looked for the destination IP address of 10.0.0.112, as this was the IP address of the target web server rather than the attacker's machine. By examining the destination ports associated with this address, I was able to identify port 8080 as the correct answer.

Port 8080 is commonly used as an alternative HTTP port for hosting web applications and services. It is frequently associated with Java application servers, including Apache Tomcat. In this case, the port provided access to the Tomcat Manager interface.

Exposed services on this port can create security risks, particularly if administrative interfaces are accessible without sufficient authentication. Attackers may exploit weak or default credentials, misconfigured services or unpatched vulnerabilities to gain unauthorised access to the server.

### Q4 - Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?

The correct answer was Gobuster. As shown in screenshot 6, the User-Agent field within the captured HTTP packets identified the tool as gobuster/3.6.

Gobuster is an open-source command-line tool commonly used by penetration testers and security professionals to discover hidden directories and files on web servers. It works by using wordlists to send requests to a target server and checking which directories or files exist.

In this investigation, the attacker appears to have used Gobuster to enumerate the web server and discover potentially accessible directories, including administrative interfaces.

### Q5 - After enumerating directories on our web server, the attacker made numerous requests to identify administrative interfaces. Which directory related to the admin panel did the attacker uncover? (Provide the path including the leading slash, e.g. /path)

As shown in screenshot 7, the correct answer was /manager.

To identify this, I filtered the captured traffic using the User-Agent value discovered in the previous question, gobuster/3.6. This allowed me to narrow down the results to requests made using Gobuster. By examining the recurring directory paths within these requests, I was able to identify /manager as the directory associated with the Tomcat administration interface.

### Q6 - After accessing the admin panel, the attacker brute-forced the login. What credentials did the attacker successfully use? (Provide them in username:password format)

To identify the credentials used to access the admin panel, I filtered the packets to find HTTP requests containing both manager and Gobuster-related traffic. I then selected one of the GET requests and expanded the HTTP Authorisation section within the packet details.

This revealed the credentials admin:tomcat, which were successfully used to authenticate with the Tomcat Manager interface.

The credentials were visible because HTTP Basic Authentication was being used. This authentication method encodes credentials using Base64 rather than encrypting them, meaning they can potentially be recovered from captured network traffic if the connection is not protected by HTTPS.

### Q7 - Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?

The correct answer was JXQOZY.war, which is displayed in screenshot 9.

To find this, I filtered the captured packets to identify HTTP POST requests. I then followed the relevant TCP stream, which allowed me to inspect the data being transferred and identify the name of the uploaded file.

A WAR file (Web Application Archive) is used to package and deploy Java web applications, particularly on servers such as Apache Tomcat. However, attackers can abuse this functionality by uploading malicious WAR files containing code designed to execute commands or establish a reverse shell on the target server.

### Q8 - After the attacker established a reverse shell on our server, the payload connects back to the attacker's machine. From the analysis, what is the callback destination in IP:port format?

As shown in screenshot 10, the callback destination was 14.0.0.120:443.

To identify this, I applied the following Wireshark display filter:

`tcp.flags.push == 1 && ip.src == 14.0.0.120 && !http`

I used this filter because I was looking for TCP traffic originating from the attacker's IP address rather than the HTTP requests involved in the earlier stages of the attack. The PSH flag can help identify packets carrying data that is being passed to the receiving application.

After applying the filter, I followed the relevant TCP stream to examine the communication between the server and the attacker's machine. This revealed the callback destination as 14.0.0.120 on port 443.

## Verdict and incident recommendations 

### Verdict

From analysing the PCAP file, it is clear that the attacker carried out a series of activities to gain unauthorised access to the web server. The attacker, using the IP address 14.0.0.120, performed an active scan against the server and discovered that port 8080 was open, providing access to the Apache Tomcat Manager interface. They then used Gobuster 3.6 to enumerate directories and successfully discovered the /manager directory. Using the credentials admin:tomcat, the attacker gained access to the admin panel and uploaded a malicious file named JXQOZY.war, which was intended to establish a reverse shell. Finally, traffic was identified connecting back to the attacker's IP address on port 443. 

### Remediation Steps

1. Change default credentials and get stronger passswords
2. Restrict access to the Tomcat Manager interface.
3. Block unnecessary ports and services.
4. Regularly update Apache Tomcat.
5. Implement firewalls and intrusion detection systems.
6. Monitor network traffic for suspicious activity.
7. Remove malicious files and investigate the compromised server.

## References + achievement link (if applicable)

https://whatismyipaddress.com/ip-lookup
https://ipfinder.app/en/learn/ip-address/how-ip-address-lookup-works/ 
https://www.researchgate.net/publication/382051703_How_dangerous_to_be_open_Ports_80 
https://www.kali.org/tools/gobuster/ 
https://nhimg.org/glossary/war-file/ 
https://cyberdefenders.org/blueteam-ctf-challenges/achievements/immy1482/tomcat-takeover/ 