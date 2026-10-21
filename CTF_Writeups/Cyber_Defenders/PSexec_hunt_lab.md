# Lab Investigation: PSexec Hunt

| Metadata | Details |
| :--- | :--- |
| **Platform** | Cyber Defenders |
| **Category** | Network Forensics |
| **Difficulty** | Easy |
| **Primary Tools Used** | `wireshark` |

---

## Incident Scenario & Objective

> **Scenario:** An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.


---

## Lab Questions & Findings

### Q1 - To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

To find a IP address the easiest, you simply go to statistic at the top, and then down to Ipv4 statistics and press on destination and source ports. This will give you a list to all the IP addresses from both the source and destination side, as shown in screenshot 1. 

In screenshot 2, it shows the screen that will pop up that gives all the addresses - from the source port side, the IP address with the highest level of activity is 10.0.0.130, making this the IP address of the attacker. 

### Q2 - To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted? 

Filtering smb2, we can trace the attacker's steps and find their host name. Pressing on the first packet and then 'follow', we can find the name of the host name - sales-pc, as shown in screenshot 3. 

The smb traffic reveals that the attakcer used NTLM. NTLM (NT LAN manager) is a Microsoft security protocol used to authenticate user identity and provide security in Windows networks. 

It works by the client sending a reqest to the server for authentication, then the server responding with a random 16-byte number (aka called 'the challenge') and then the client will encrypt the challenge using a one-way hash of the user's password and sends the response back. The server/domain will then check the result against the stored hash to confirm the identity. 


### Q3 -

### Q4 -

### Q5 -

### Q6 -

### Q7 -

## Verdict and incident recommendations 

### Verdict

### Remediation Steps

## References + achievement link (if applicable)