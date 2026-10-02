# XXE Infiltration Lab Writeup

| Field | Details |
| :--- | :--- |
| **Category** | [Network Security] |
| **Difficulty** | [Easy] |
| **Lab Link** | [https://cyberdefenders.org/blueteam-ctf-challenges/xxe-infiltration/] |
| **Tools Used** | `Wireshark, Brim` |

---

## Summary

> **Scenario Overview:** There was a automated alert detected suggestting a XXE. Using the tools available, I identified how the attacker gained access and what actions they took. 
> **Note:** To comply with CyberDefender's policy, I won't be putting the answers in this writeup. 

---

## Step-by-step Investigation and Analysis

### Q.1: During the attacker's port scan, what is the highest-numbered TCP port that responded as open on the victim host? 

Basically, what I did was filter the packets with a SYN flag and ACK, so:

`tcp.flags.syn == 1 && tcp.flags.ack == 1`

Once that's filtered, just have a look at the info - naturally, the one with the largest port number was first. I *did* attempt to get a column with the source ports, but unfortunately wireshark didn't seem to agree with me. 

### Q.2 By identifying the vulnerable PHP script, security teams can directly address and mitigate the vulnerability. What’s the complete URI of the PHP script vulnerable to XXE Injection?

Right. Filtering it again was easy, though, this took a little long for me to find where the uri is - and then when I did, I must have not inputted it right because it rejected my answer...

Ah well. After following one of the packets, I found the attacker sending a POST request including a malicious XML file, which was - what seems to be the theme in this lab - the name of a famous book, *ToKillAMockingBird.xml*. As the lab goes on, I found that there were multiple references to famous books, which made this reader's heart very happy!

Anyway, after realising (using the format provided by CyberDefenders) what the correct answer was. 

### Q.3 To construct the attack timeline and determine the initial point of compromise. What's the name of the first malicious XML file uploaded by the attacker? 

After looking up `tcp.stream eq 10459`, and following the fourth packet, (since it was not like the others in it's info) I found the name of the file! 

(See second screenshot in folder)

### Q.4 Understanding which sensitive files were accessed helps evaluate the breach's potential impact. What's the name of the web app configuration file the attacker read?

The naming for this one wasn't so creative as the previous ones, unfortunately. After finding the previous answer, I looked at what the filter I should be using, `tcp.stream eq 10462` and followed the packed with .php in the name - since we know from earlier the vunlerability is in a .php endpoint. 

(Go to screenshot 6!)

### Q.5  To assess the scope of the breach, what is the password for the compromised database user?

Unfortunately, the password for this isn't as complicated as you'd hope it would be. 

Thankfully, I didn't need to leave the packet for this one, 

The user is 'pageturner' which makes sense given the theme.

### Q.6 After stealing the credentials from the config file, the attacker authenticates to the MySQL service on the victim. Using the Wireshark filter mysql.login_request, what is the timestamp (UTC) of the attacker's first MySQL login attempt?

After using the filter recommended, I looked at the time column and then the answer asked and realised that I couldn't submit the time as it is by default in wireshark. So, I simply edited the column's formatting into YYYY-MM-DD and time.

That was probably the easiest question so far! And we're nearly at the end!

(See screenshot 4)

### Q.7 To eliminate the threat and prevent further unauthorized access, can you identify the name of the web shell that the attacker uploaded for remote code execution and persistence?

Afterward, I filtered it by XML and POST and in the packets, in the information, was exactly the answer I need to find! (See screenshot 5!)

## Closing thoughts and remarks

For my first lab outside of university (and being *wireshark* which I haven't touched since first semester..) I thought that it was surprisingly easy to wrap my head around. Some questions were easier than others, but I enjoyed doing it. 

Lessons learned:

- Importance of filtering and how much time it saves (I only had an hour to complete this lab before it shuts down on me, so I was under pressure there)
- Improved my skills on using wireshark - and how to find certain bits of information despite a lot of it looking similarly to me at times
- And generally reinforced things I have covered in my university studies.

Thanks for reading! 