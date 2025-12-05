# Creating Lookups

Created a csv file called vip-users.csv
<img width="239" height="178" alt="image" src="https://github.com/user-attachments/assets/8a7bf7c9-1c39-4b66-b117-fa186d7b5565" />

Opened lookups in Splunk

<img width="350" height="319" alt="image (1)" src="https://github.com/user-attachments/assets/d7cb5116-888e-4724-8645-a85c693aa53f" />


<img width="953" height="331" alt="image (2)" src="https://github.com/user-attachments/assets/ffaaf6c3-ce02-44c3-839f-4b0653a5d9e6" />

Now click on New Look Up table File

Uploaded the csv file into Splunk


<img width="2452" height="573" alt="image (3)" src="https://github.com/user-attachments/assets/0f2fc37c-7f26-488b-9fa8-82ded920de6b" />


<img width="1094" height="1088" alt="image (4)" src="https://github.com/user-attachments/assets/ac7d7be5-f492-4d08-ba7f-13a317c52140" />

Go to search and change the date range from 01/07/2024 to 01/08/2024
```bash
index="mydfir-soc" sourcetype="Linux:auth" host="splunk" source="auth.csv"
```

<img width="2423" height="1117" alt="image (5)" src="https://github.com/user-attachments/assets/bb89c1fa-fd1c-4692-9568-e5e6d79a2b67" />

Filtering only per root user
```bash
index="mydfir-soc" sourcetype="Linux:auth" host="splunk" source="auth.csv" user=root
```


<img width="2423" height="1117" alt="image (6)" src="https://github.com/user-attachments/assets/514f9a20-f701-4ad9-ad81-e07d95e76604" />

Now we can invoke our look up table (csv) created usin the “lookup” in the search

```bash
index="mydfir-soc" sourcetype="Linux:auth" host="splunk" source="auth.csv" user=root | lookup vip-users.csv user OUTPUT user as vip_user title as position
```
There are two new fields: in the left panel:

“position” with a value: administrator

“vip_user” with a value:  root


<img width="860" height="242" alt="image (7)" src="https://github.com/user-attachments/assets/f58a5dc5-3b08-483c-b4f2-a5c0d20da55e" />

<img width="856" height="264" alt="image (8)" src="https://github.com/user-attachments/assets/d144a42c-8c1a-4c4c-b062-0a490cb12d6f" />

Conclusion:

A lookup table can help to obtain additional context during search time.
Also we might use threat intelligence or list of legitimate processes to build a baseline full of legitimate processes within my environment and then utilizing a lookup table to basically whitelist them to find suspicious or malicious activity.














