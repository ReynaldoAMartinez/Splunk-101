# **Scenario:**

Ryan Adams, a local administrator at a small dental clinic, contacted the **MahCyberDefense** **SOC** hotline after suspecting his computer may have been compromised. He explained that around **October 15 2025 at 13:00 UTC**, his mouse was randomly moving so he became suspicious.

As a SOC analyst, your task is to investigate this incident using Splunk. Analyze the available data to determine what occurred, identify any indicators of compromise, and document your findings in a SOC report using the provided format found in the community.

# Solution

Since we only have short information such as user, date & time, I started running a simple query for the time range between Oct 14, 2025 and Oct 16, 2025.


<img width="655" height="153" alt="image" src="https://github.com/user-attachments/assets/820fa44a-38e6-4993-905e-59344ad97322" />

Searching and filtering by user Ryan Adams

```bash
index=* user="Ryan.Adams"
```
I found 1 host (FRONTDESK-PC1), 2 user (Ryan.Adams, ryan.adams), 6 DestinationIp, and 23 EventCode.
Only one external IP 157[.]245[.]46[.]190


<img width="528" height="251" alt="image (1)" src="https://github.com/user-attachments/assets/3d896f0b-9997-419d-959a-ae544bd8c375" />


<img width="528" height="251" alt="image (2)" src="https://github.com/user-attachments/assets/c0ab6267-e210-4355-b553-ec66fa406163" />


<img width="538" height="369" alt="image (3)" src="https://github.com/user-attachments/assets/8c0381fd-18de-4650-b9ac-80c3dd247072" />


<img width="529" height="493" alt="image (4)" src="https://github.com/user-attachments/assets/372cc3d6-c168-4c36-82dd-1c8475e96f2c" />

Checked for "Successful logins (EventCode 4624)"

```bash
index=* EventCode=4624 host=FRONTDESK-PC1  user="Ryan.Adams" 
| table  _time, user, src_ip, Logon_Type, host  
| sort  _time
```

<img width="2092" height="483" alt="image (5)" src="https://github.com/user-attachments/assets/a7152dcc-3be7-4186-b56f-07c21afc66a1" />

It shows a source IP and the Logon Types for the successful logins

src_ip = 172.16.0.184

Logon_Type
3 --> Network --> SMB file share access, lateral movement. -->  The logon occurred over the network, not locally.
7 --> Unlock --> Workstation unlock --> The user unlocked a workstation that was already logged in.

Checked for Failed Login Attempts (EventCode 4625)

```bash
index=* EventCode=4625 
| stats  count by user, src_ip  
| where  count > 5 
| sort  -count
```

<img width="2430" height="352" alt="image (6)" src="https://github.com/user-attachments/assets/f05cf325-4418-4106-8895-8c4d634067e1" />

Checked for "Logon with Explicit Credentials --> Lateral movement (RunAs, PsExec)"

```bash
index=* EventCode=4648 host=FRONTDESK-PC1  user="Ryan.Adams" 
| table  _time, user, Target_User_Name, Target_Server_Name, src_ip, Logon_Type, host    
| sort  _time
```
<img width="2430" height="352" alt="image" src="https://github.com/user-attachments/assets/f7fdeec4-0cea-47d8-8d79-a46ca60c6b4c" />

Checked for "Special Privileges Assigned --> Admin logons, privilege escalation"

```bash
index=* EventCode=4672 host=FRONTDESK-PC1 user="Ryan.Adams" 
|table _time, user, Privileges
```

<img width="2408" height="405" alt="image (1)" src="https://github.com/user-attachments/assets/8603bf57-7a3f-43dd-abc4-2191194e68e2" />

SeSecurityPrivilege --> is a Windows user right (privilege) that allows an account to manage auditing and security logs.

Checked for "PowerShell Script Block Logging - PowerShell-based attacks"

```bash
index=endpoint EventCode=4104  host="FRONTDESK-PC1" 
| table  _time,LogName, Message
```

<img width="1705" height="1125" alt="image (2)" src="https://github.com/user-attachments/assets/cb04eeb4-8bbd-49ba-ab8c-fbb3884d38e9" />

<img width="1661" height="244" alt="image (3)" src="https://github.com/user-attachments/assets/4c0b0f0a-611e-4af1-b2ec-12d946109fa7" />

schtasks.exe /create /tn "PythonUpdate" /tr "C:\Users\Ryan.Adams\Music\

python.exe" /sc onstart /ru SYSTEM /f

This sets up a task that runs python.exe from Ryan.Adams’ Music folder every time the computer starts, with SYSTEM-level privileges.

Checked fot "Track Process Execution (Sysmon Event ID 1)"

```bash
index=* host=FRONTDESK-PC1 EventCode=1 user="Ryan.Adams" 
|table  _time, Image, CommandLine, ParentImage, user, src_ip, Account_Name,  Logon_Type, host 
|sort _time
```

<img width="2431" height="1128" alt="image (4)" src="https://github.com/user-attachments/assets/323a3b08-e3f6-4706-b9cb-bfcb9a758928" />

Checked for "Find Network Connections (Sysmon Event ID 3)"

```bash
index=* EventCode=3 host=FRONTDESK-PC1  
| where NOT (DestinationIp="10.*" OR DestinationIp="172.*" OR DestinationIp="192.168.*")   
| search user="Ryan.Adams" 
| table  _time, Image, DestinationIp, DestinationPort, DestinationHostname   
| sort  _time desc
```

<img width="2192" height="446" alt="image (5)" src="https://github.com/user-attachments/assets/c5244a7c-892c-4956-8804-2344a88200ba" />

157[.]245[.]46[.]190 --> This IP was reported 106 times (AbuseIPDB)

<img width="713" height="758" alt="image (6)" src="https://github.com/user-attachments/assets/0090150c-2567-4405-8e71-df67f3d1ec53" />

<img width="1353" height="820" alt="image (7)" src="https://github.com/user-attachments/assets/ba7c6ef7-c990-4ddf-8872-5d9a669a5f1d" />

Also Virustotal reported the IP as malicious

<img width="1622" height="746" alt="image (8)" src="https://github.com/user-attachments/assets/e5564a25-a9e6-4916-8329-9801f31029fd" />

<img width="638" height="928" alt="image (9)" src="https://github.com/user-attachments/assets/07de85fa-0fa3-473c-9b1c-5f885a1cf7e4" />

The IP is linked to the malware Sliver C2 at 157[.]245[.]46[.]190[:]8888   and  a threat type: botnet_cc

<img width="1133" height="482" alt="image (10)" src="https://github.com/user-attachments/assets/d700da3d-3a62-4134-bad9-d5a492f78695" />


Checked for "Files Created (Sysmon Event ID 11)"

```bash
index=endpoint EventCode=11 host=FRONTDESK-PC1 TargetFilename="*temp*"   user="Ryan.Adams" 
| table  _time, Image, TargetFilename, User   
| sort  _time desc
```

<img width="2402" height="571" alt="image (11)" src="https://github.com/user-attachments/assets/9d78ef49-7e50-4c1b-b20c-f885bcdac40f" />

Checked for "Registry Persistence Checks (Sysmon Event ID 13)"

```bash
index=endpoint EventCode=13 host=FRONTDESK-PC1 TargetObject="*Run*"
| table _time, Image, TargetObject, Details, User
| sort _time desc
```

<img width="2420" height="318" alt="image (12)" src="https://github.com/user-attachments/assets/17bbbec7-cff8-43eb-ad58-1611bbc63791" />

It launches Microsoft Edge during Windows login/startup, often to resume the last browsing session or display the default start page.

**SOC Investigation Report - Kerning City Dental (KCD)**

**Incident:**

Suspicious Remote Activity on BACKOFFICE-PC1

**Reported by:**

Ryan Adams (CEO & Lead Dentist)

**Date of Report:**

October 2025


# **Findings (What did you find)**

- A total of **66 failed login attempts (EventCode 4625)** were generated from the user account **Ryan.Adams** on **BACKOFFICE-PC1**.
- This high number of authentication failures strongly suggests a **Brute Force (T1110)** or **Password Spraying attempt**, depending on whether multiple passwords or a single password was used.
- A sequence of **privileged logons**, **lateral movement**, **remote process execution**, and **persistence creation** occurred on **Ryan Adams’ computer (BACKOFFICE-PC1)**.
- The attacker authenticated from **FRONTDESK-PC1** to **BACKOFFICE-PC1** using Ryan Adams’ credentials.
- A suspicious binary **python.exe** located in Ryan’s Music folder was executed.
- The Python process made an outbound connection to **157.245.46.190:8888**, a non-KCD external IP.
- PowerShell activity showed directory changes and scriptblock creation.
- A **scheduled task named “PythonUpdate”** was created to execute the malicious python binary with **SYSTEM** privileges.
- Strong indicators of **remote control**, **malware execution**, and **persistence** were found.

# **Investigation Summary (What happened)**

On **October 15, 2025**, shortly before 13:00 UTC, Ryan Adams noticed unexplained mouse movements on BACKOFFICE-PC1. Splunk logs confirm that his credentials were used minutes earlier for **lateral movement** from FRONTDESK-PC1.

Immediately after the connection, a malicious **python.exe** file in Ryan’s Music folder was executed and connected to an external command-and-control (C2) server. PowerShell commands were run, followed by creating a scheduled task that would maintain persistent access for the attacker.

Timeline indicates a classic compromise involving:

1. Privilege escalation
2. Credential abuse
3. Remote logon
4. Malware execution
5. Network beaconing
6. Persistence creation

No further malicious activity after October 15th 13:04 UTC was found, but the persistence mechanism would have allowed continued access until removed.

# **Who, What, When, Where, Why, How**

## **Who – Who was involved?**

- **Victim:** Ryan Adams (BACKOFFICE-PC1)
- **Attacker:** Unknown (used Ryan’s credentials; likely external actor)
- **Systems involved:**
    - **FRONTDESK-PC1** (source of lateral movement)
    - **BACKOFFICE-PC1** (compromised target)
 
## **What – What happened?**

Before the attacker successfully authenticated with Ryan Adams’ account (EventCode 4624), there were 66 failed login attempts. This strongly indicates: Password guessing / brute-force activity

The attacker was likely attempting to guess Ryan’s password repeatedly until they succeeded.

Targeted credential attack. The repeated failures specifically against one user account (Ryan.Adams) suggests intentional targeting. The successful login that followed is almost certainly unauthorized.

This strengthens the assessment that Ryan Adams’ credentials were compromised through password spraying / brute force, not legitimate activity.

- Unauthorized use of Ryan Adams’ credentials
- Lateral movement from FRONTDESK-PC1 → BACKOFFICE-PC1
- Execution of suspicious Python malware
- Outbound C2 connection
- PowerShell scriptblock creation
- Creation of persistence via Windows Scheduled Task (“PythonUpdate”)

## **When – When did this occur and is it still happening?**

### **Timeline (UTC)**

- **12:55:12** – Privilege logon (4672)
- **12:55:20** – Successful remote login to FRONTDESK-PC1 (4624)
- **12:55:20** – Explicit credentials used for lateral movement (4648)
- **13:00:33** – Execution of malicious `python.exe`
- **13:00:34** – Outbound network connection to **157[.]245[.]46[.]190[:]8888**
- **13:00:51** – PowerShell execution
- **13:04:59** – Scheduled task created (“PythonUpdate”)

**Is it still happening?**

- No additional C2 connections detected after 13:04.
- However, the **persistence task would continue running** unless manually removed.

## **Where – Where in the environment did this happen?**

- **Target system:** BACKOFFICE-PC1
- **Source system:** FRONTDESK-PC1
- **Directory of malicious binary:**
    
    `C:\Users\Ryan.Adams\Music\python.exe`
    
- **External connection:**
    
    `157[.]245[.]46[.]190[:]8888`

## **Why – Why did this happen? (If known)**

Most likely due to:

- Credential theft or phishing of Ryan Adams
- The attacker using compromised credentials to move laterally
- Execution of malware designed to provide remote interactive access
- Persistence creation to maintain long-term access

Root cause: **Credential compromise + inadequate privileged access controls**

## **How – How did this happen?**

1. Attacker obtained Ryan Adams’ credentials - **Brute Force Multiple authentication failures attempting to guess a user's password.**
2. Logged into FRONTDESK-PC1
3. Used **Event 4648** explicit credentials to laterally move to BACKOFFICE-PC1
4. Executed malicious Python binary
5. Python connected to remote C2 server
6. PowerShell launched to adjust directories and execute commands
7. Created a SYSTEM-level scheduled task to maintain persistence

The unexpected mouse movement observed by Ryan likely came from attacker remote control.

# **Recommendations**

### **Immediate Remediation**

- **Reset Ryan Adams’ password** and invalidate active sessions.
- **Disable and delete the “PythonUpdate” scheduled task.**
- **Delete the malicious python.exe file** at:
    
    `C:\Users\Ryan.Adams\Music\python.exe`
    
- **Block outbound traffic** to **157[.]245[.]46[.]190** at perimeter firewall.
- **Reimage BACKOFFICE-PC1** if malware integrity is uncertain.

### **Detection & Monitoring**

- Add detections for:
    - Scheduled task creation
    - PowerShell scriptblock logging anomalies
    - Process execution from unusual directories (Music folder)
    - Outbound connections to high-risk ports (e.g., 8888)

### **Prevention**

- Enforce **MFA on all accounts**
- Limit lateral movement by reducing **local admin privileges**
- Deploy endpoint protection that blocks unsigned/unknown Python executables
- Implement **credential guard** and **LSASS protection**
- Conduct security awareness training about phishing attacks



