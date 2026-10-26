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

### Q1: To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

PsExec is a lightweight command-line tool that allows users to run programs on remote systems. It can execute commands, scripts and applications. In this investigation, we will examine suspicious activity involving PsExec.

To identify the IP address, I navigated to Statistic in Wireshark, followed by IPv4 Statistics. This provided a list of IP addresses involved in the captured network traffic, as shown in Screenshot 1.

Screenshot 2 shows the list of IP addresses and their associated traffic statistics. The IP address with the highest level of activity on the source side was 10.0.0.130, making it the suspected attacker machine.

### Q2: To fully understand the extent of the breach, can you determine the hostname of the machine to which the attacker first pivoted?

By filtering the traffic using smb2, I was able to trace the attacker's activity and identify the hostname of the target machine. By selecting the first relevant packet and following the associated traffic, I identified the hostname as SALES-PC, as shown in Screenshot 3.

The SMB traffic also revealed that NTLM authentication was used. NTLM (NT LAN Manager) is a Microsoft authentication protocol used to verify user identities in Windows networks.

It uses a challenge-response mechanism. The server sends the client a random challenge, which the client processes using information derived from the user's password. The client then sends the response back to the server, which verifies it against the expected result. This allows the server to authenticate the user without sending the password directly over the network.

### Q3: Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What username did the attacker use for authentication?

PsExec uses SMB to establish connections to remote systems, and the associated authentication traffic can reveal the account used. Screenshot 4 shows the username in the packet details: ssales.

This suggests that the attacker used the ssales account to authenticate to the target system. Identifying the account helps the incident response team investigate potential credential compromise, determine the account's privileges and assess whether it was used to access other systems.

### Q4: After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What is the name of the service executable the attacker set up on the target?

I analysed the SMB2 requests and identified a file creation request associated with the installation of the PsExec service on the target machine.

As shown in Screenshot 5, the service executable was named PSEXESVC.exe. This executable is associated with PsExec, a legitimate Windows administration tool that can be abused by attackers to execute commands remotely.

In this case, its use indicates that the attacker was attempting to establish remote command execution on the target machine, supporting the evidence of lateral movement.

### Q5: We need to know how the attacker installed the service on the compromised machine to understand their lateral movement tactics. Which network share did PsExec use to install the service on the target machine?

By examining the SMB2 requests associated with PsExec, I identified the network share used to install the service as ADMIN$, as shown in Screenshot 6.

The ADMIN$ share is a Windows administrative share that provides access to the Windows directory on a remote system. PsExec can use this share to copy its service executable to the target machine before starting the service.

The use of this share indicates potential lateral movement through administrative access and valid or compromised credentials.

### Q6: We need to identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

The attacker used the IPC$ network share for communication, as shown in Screenshot 7, where IPC$ appears in the packet information.

IPC stands for Inter-Process Communication. In Windows, the IPC$ share supports communication between processes over the network, including through named pipes used by administrative services and remote management tools.

The captured traffic indicates that the SMB communication was associated with the ssales account. In this investigation, the use of IPC$ is consistent with PsExec's remote communication and service management activities.

### Q7: Now that we have a clearer picture of the attacker's activities on the compromised machine, it is important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted?

Screenshot 8 identifies the second target as MARKETING-PC. However, Screenshot 9 shows a STATUS_LOGON_FAILURE response, indicating that the authentication attempt failed.

This suggests that the attacker attempted to access another machine using credentials that were not accepted. Although this particular attempt appears to have been unsuccessful, it demonstrates an attempt to expand access across the network.

## Verdict and incident recommendations 

### Verdict

The investigation identified 10.0.0.130 as the suspected attacker machine, which accessed SALES-PC using the ssales account. The attacker used PsExec to perform lateral movement over SMB2, installing the PSEXESVC.exe service executable through the ADMIN$ administrative share and using IPC$ for communication. The attacker then attempted to access MARKETING-PC, but the attempt resulted in a STATUS_LOGON_FAILURE, indicating that authentication was unsuccessful. The findings suggest that SALES-PC was potentially compromised and that the attacker attempted to expand their access across the network. Further investigation would be required to determine whether additional systems were compromised, what commands were executed and whether any sensitive data was accessed or exfiltrated.

### Remediation Steps

1. Isolate affected systems - disconnect SALES-PC from the network while preserving forensic evidence for further investigation.
2. Secure compromised credentials - Reset the ssales account password, review its privileges and investigate potential credential reuse.
5. Restrict administrative access - limit access to ADMIN$ and IPC$ and use firewall rules and network segmentation to restrict unnecessary SMB connections.
6. Strengthen authentication - Enforce strong, unique passwords and multifactor authentication where supported, and reduce reliance on NTLM where feasible.
7. Improve monitoring - Configure security tools to detect unusual PsExec usage, suspicious service creation and repeated authentication failures.

## References + achievement link (if applicable)

https://www.silverfort.com/glossary/psexec/ 
https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/inter-process-communication-share-null-session 
https://cyberdefenders.org/blueteam-ctf-challenges/achievements/immy1482/psexec-hunt/ 
