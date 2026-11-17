# Lab Investigation: L'Espion


| Metadata | Details |
| :--- | :--- |
| **Platform** |Cyberdefenders |
| **Category** | Threat Intel |
| **Difficulty** | Easy |
| **Primary Tools Used** | `cyberchef`, `google maps`, `github` |


---


## Incident Scenario & Objective


> **Scenario:** Investigate an insider threat by analyzing GitHub repositories for exposed credentials, using OSINT tools to correlate online accounts, and performing image analysis to identify locations.


---


## Lab Questions & Findings


### Q1 - File -> Github.txt: What API key did the insider add to his GitHub repositories? 


Hello!

Today, the lab we are completing is a bit different from the others. This time, we are doing some google mapping and looking at a github, which I haven’t really done since first year at uni. 


So, lets get started!


Well, to find the API key, I decided to look around github (oh, really?) and clicked on a project that Marsielle made. In one of her pages, called ‘login page.js’, it states clearly at the top the API key! As shown in screenshot 1!




### Q2 - File -> Github.txt: What plaintext password did the insider add to his GitHub repositories?
Scrolling down a little on the same page, I did find a user name and password. But, unfortunately, the original was in base64, as preference below the password itself. Shown in screenshot 2.


Immediately, I thought of my dear old friend ‘cyberchef’ and was able to translate it. The answer turned out to be PicassoBaguette99 - how delicious. This is shown in screenshot 3.  


### Q3 - File -> Github.txt: What cryptocurrency mining tool did the insider use?


So, I went and looked around on their repo again and saw they had a project called ‘xmrig’, as shown on screenshot 4, which is a cryptocurrency mining software.


It’s open source, and primarily designed to mine Monero (XMR) and other Proof-of-Work coins using algorithms. In intended use, it’s a legal, transparent utility that cryptocurrency guys run on their CPUs and GPUs to gain mining rewards. However, because it’s very efficient, it’s also frequently hijacked and bundled with malware and trojans. It can be used for cryptojacking, which is when it’s installed somewhere without the owner’s permission - and then it silently consumes CPU and GPU resources to mine crypto. 


### Q4 - On which gaming website did the insider have an account? 


On screenshot 5 it shows their steam account. All I really had to do was search up the username that they have on github and it shows up. 


### Q5 - What is the link to the insider Instagram profile?


Because I conducted a google search, it also brought up her instagram page as shown in screenshot 6. It is https://www.instagram.com/emarseille99/?hl=en. 




### Q6 - Where did the insider go on the holiday? (Country only)


To find this out, I went onto her Instagram and went through her pictures - of which there were only 6, as shown in screenshot 7. On one of them, Marsille wrote “Once in a lifetime holiday here, love me some slings x”. So, that one must be where she went.


So, I put it into Pictarta, which is a free image location finder online. The result was Singapore, as shown in screenshot 8.


### Q7 - Where is the insider family live? (City only)


I know Marsille doesn’t exist, but now I was starting to feel like a bit of a creep here.


Another one of Marsielle’s posts states “Nice to meet friends & family Photo ½” with a picture of a house. Using the same software, it depicted the photo being taken in Dubai as shown in screenshot 9.  


… But even then, it was obvious by the next photo in the colleague as the Burj Khalifa was in the background. 


### Q8 - File -> office.jpg: You have been provided with a picture of the building in which the company has an office. Which city is the company located in?


In the folder provided, there were two photos - one called office.jpg and another called webcam.png. Both are in the screenshot folder for this lab for you to take a look at.


So, in Office.jpg, it was mostly focused on signs which had theatres, churches, markets and other such things on them. In the background, there was clearly a sign that said ‘GRAND CENTRAL’ so that made me think of some kind of train station.


I searched up St. Martin’s church and then ‘grand central’ on the end, and found that it was located in Birmingham, as shown in screenshot 10. 




### Q9 -   File -> Webcam.png: With the intel, you have provided, our ground surveillance unit is now overlooking the person of interest suspected address. They saw them leaving their apartment and followed them to the airport. Their plane took off and has landed in another country. Our intelligence team spotted the target with this IP camera. Which state is this camera in?


The webcam hardly gave me clues as much as the office picture did, but I decided to use google reverse imaging and found that it was the University of Notre Dam, which is located in Indiana, USA.


## Verdict and incident recommendations


## Verdict and incident recommendations


### Verdict


This lab shows a credible insider threat, built from three findings:


The insider committed a live API key and a base64-encoded username and password to a public GitHub repository. Base64 is encoding, not encryption, so anyone could decode it in seconds with CyberChef. The insider had an xmrig repository. XMRig is legitimate open-source mining software, but it is commonly used for cryptojacking. A repository alone does not prove it ran on company systems, so host and network evidence (CPU spikes, connections to mining pools) would be needed to confirm it. The same username linked their GitHub, Steam and Instagram accounts. Their posts revealed a holiday location (Singapore) and where their family lives (Dubai). Combined with the office photo (Birmingham, UK) and the webcam image (Indiana, USA), this lets an attacker or an investigator build a detailed profile of the person.


### Remediation Steps


1. **Revoke and rotate** the exposed API key and reset the exposed account password immediately. Assume both are compromised.
2. **Check the logs:** Review API and authentication logs for use of the leaked credentials from unfamiliar IPs or at unusual times.
3. **Remove secrets from the repository and its history** (e.g. with `git filter-repo` or BFG). Deleting the file is not enough, because old commits still contain the secret.
4. **Hunt for cryptomining:** Look for XMRig binaries and processes, sustained high CPU/GPU use, and DNS or network connections to known mining pools. Block those pools and add EDR detections.
5. **Prevent future leaks:** Enable GitHub secret scanning and push protection, add pre-commit scanning (gitleaks or truffleHog), and store credentials in a secrets manager rather than in code.
6. **Handle the insider properly:** Involve HR and legal, restrict their access to least privilege, and preserve evidence.
7. **Policy and training:** Set clear rules on personal GitHub use and company code, and run awareness training on credential hygiene and the OSINT risks of reusing usernames across sites.




## References + achievement link (if applicable)


https://xmrig.com
https://www.checkpoint.com/cyber-hub/threat-prevention/what-is-malware/xmrig-malware/ 
https://cyberdefenders.org/blueteam-ctf-challenges/achievements/immy1482/lespion/
 
