# **Scenario:**

Ryan Adams, a local administrator at a small dental clinic, contacted the **MahCyberDefense** **SOC** hotline after suspecting his computer may have been compromised. He explained that around **October 15 2025 at 13:00 UTC**, his mouse was randomly moving so he became suspicious.

As a SOC analyst, your task is to investigate this incident using Splunk. Analyze the available data to determine what occurred, identify any indicators of compromise, and document your findings in a SOC report using the provided format found in the community.

# Solution

Since we only have short information such as user, date & time to investigate, we will start running a SPL with a simple query. We set a date & time range between Oct 14, 2025 at midnight and Oct 16, 2025 at midnight to explore a wide period.


<img width="655" height="153" alt="image" src="https://github.com/user-attachments/assets/820fa44a-38e6-4993-905e-59344ad97322" />

Searched by user Ryan Adams

```bash
index=* user="Ryan.Adams"
```
We found 1 host (FRONTDESK-PC1), 2 user (Ryan.Adams, ryan.adams), 6 DestinationIp, and 23 EventCode


<img width="528" height="251" alt="image (1)" src="https://github.com/user-attachments/assets/3d896f0b-9997-419d-959a-ae544bd8c375" />


<img width="528" height="251" alt="image (2)" src="https://github.com/user-attachments/assets/c0ab6267-e210-4355-b553-ec66fa406163" />


<img width="538" height="369" alt="image (3)" src="https://github.com/user-attachments/assets/8c0381fd-18de-4650-b9ac-80c3dd247072" />


<img width="529" height="493" alt="image (4)" src="https://github.com/user-attachments/assets/372cc3d6-c168-4c36-82dd-1c8475e96f2c" />

Checked for successful logins (EventCode 4624)

```bash
index=* EventCode=4624 host=FRONTDESK-PC1  user="Ryan.Adams" 
| table  _time, user, src_ip, Logon_Type, host  
| sort  _time
```

<img width="2092" height="483" alt="image (5)" src="https://github.com/user-attachments/assets/a7152dcc-3be7-4186-b56f-07c21afc66a1" />

I found source IP and the Logon Type for the successful logins

src_ip = 172.16.0.184

Logon_Type
3 --> Network --> SMB file share access, lateral movement. -->  The logon occurred over the network, not locally.
7 --> Unlock --> Workstation unlock --> The user unlocked a workstation that was already logged in.

Failed Login Attempts (EventCode 4625)

```bash
index=* EventCode=4625 
| stats  count by user, src_ip  
| where  count > 5 
| sort  -count
```

<img width="2430" height="352" alt="image (6)" src="https://github.com/user-attachments/assets/f05cf325-4418-4106-8895-8c4d634067e1" />













































