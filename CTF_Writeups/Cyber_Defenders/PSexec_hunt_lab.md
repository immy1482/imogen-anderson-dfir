# Lab Investigation: PSexec Hunt

| Metadata | Details |
| :--- | :--- |
| **Platform** | Cyber Defenders |
| **Category** | Network Forensics |
| **Difficulty** | Easy |
| **Tactics** | Executon, defense impairment, discovery, lateral movement |
| **Primary Tools Used** | `wireshark` |

---

## Incident Scenario & Objective

> **Scenario:** An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.


---

## Lab Questions & Findings

### Q1 - To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

*PSexec* is a light-weight command line tool that users use to run programs while being on remote systems. It can execute commands, scripts and applications. Today, we'll be investigating suspicious activity that involves PSexec!

To find a IP address the easiest, you simply go to statistic at the top, and then down to Ipv4 statistics and press on destination and source ports. This will give you a list to all the IP addresses from both the source and destination side, as shown in screenshot 1. 

In screenshot 2, it shows the screen that will pop up that gives all the addresses - from the source port side, the IP address with the highest level of activity is 10.0.0.130, making this the IP address of the attacker. 

### Q2 - To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted? 

Filtering smb2, we can trace the attacker's steps and find their host name. Pressing on the first packet and then 'follow', we can find the name of the host name - sales-pc, as shown in screenshot 3. 

The smb traffic reveals that the attakcer used NTLM. NTLM (NT LAN manager) is a Microsoft security protocol used to authenticate user identity and provide security in Windows networks. 

It works by the client sending a reqest to the server for authentication, then the server responding with a random 16-byte number (aka called 'the challenge') and then the client will encrypt the challenge using a one-way hash of the user's password and sends the response back. The server/domain will then check the result against the stored hash to confirm the identity. 


### Q3 - Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

PSexec embeds the authentication details within the SMB traffic. Screenshot 4 shows the user clearly outlined in the info of the packet. The user being 'ssales', similar to sales-pc. 

This clearly shows that the attakcer had compromised and used the user 'ssales' to gain access into the target system. This can help the incident response teams to assess the damage now that the user associated with the attack has been identified. 

### Q4 - After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

So, I analyse further the smb2 requests and found that the attacker made a create request through SMB to set up the executable on the target. 

In screenshot 5, it clearly shows the name of the service - PSEXESVC.exe. This executable was used with PsExec, which as explained before, is a legitmate tool used in Windows PCs but misused and abused here. 

By using it here, the attakcer can execute commands remotely and effectively compromise the host. 


### Q5 - We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?

In the tree ID of the SMB2 requests with the PSexec, the network share that was used was identified as ADMIN$, as show in screenshot 6. 

Using the ADMIN$ share allows the attacker to remotely copy the PsExec service executable to the target machine and gain the remote access they desire. This indicates their lateral movement since it relies on accessing privileged shares and using valid credentials to execute their commands on systems. 

### Q6 - We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

For communication, the attacker used the IPC$ network share, as shown in screenshot 7 where the IPC$ was in the info of some of the packets.

IPC is short for Inter-Process Communication. It' allows anonymous users perform certain activities on a Windows PC. It usually used by administrators who are running remote management tasks and commands. 

IPC is common in SMB communications particularly for these reasons. The connection attributes in the packet indicate the session was established using the ssales account and the communication then proceeded over SMB2. 

### Q7 - Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?



## Verdict and incident recommendations 

### Verdict

### Remediation Steps

## References + achievement link (if applicable)

https://www.silverfort.com/glossary/psexec/ 
https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/inter-process-communication-share-null-session 

