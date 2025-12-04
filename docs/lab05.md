# SPL & Transforming Commands

Next are the basic fields using in a start search in Splunk

```bash
index="mydfir-soc" sourcetype=Linux:auth host=splunk source="auth.csv"
```

Using the Job bottom I can confirm my query
<img width="1466" height="712" alt="image" src="https://github.com/user-attachments/assets/7d238c15-1f4c-4f82-8cdf-3507fa3c67b3" />

the fields are case sensitive; the values are not case sensitive

the three modes

<img width="319" height="257" alt="image (1)" src="https://github.com/user-attachments/assets/2fd82c7b-9088-45a5-9736-337d32318566" />

Fast mode shows less fields on the left panel because it focuses on speed
If I am interested in one specific field , I can use a SPL like this one

```bash
index="mydfir-soc" sourcetype=Linux:auth host=splunk source="auth.csv" | stats count by host
```

<img width="2294" height="541" alt="image (2)" src="https://github.com/user-attachments/assets/b9634f33-f754-431e-8800-58be7ae895a4" />

If I add one more field like index, I can see both now

<img width="705" height="506" alt="image (3)" src="https://github.com/user-attachments/assets/54017377-ea87-49a7-8c80-467bdc0c1886" />

What I learned is the purpose of “fields” is performance.

Next is Table

<img width="1541" height="869" alt="image (4)" src="https://github.com/user-attachments/assets/70390c09-49b7-45f1-8b7f-13b925745017" />

Next one is a table listed by time

<img width="622" height="891" alt="image (5)" src="https://github.com/user-attachments/assets/b9a7eaaa-dd9e-42ab-968a-e7c39b85891f" />

Next SPL using the "where" command

<img width="1014" height="832" alt="image (6)" src="https://github.com/user-attachments/assets/4a7e0a92-59fb-472e-95c5-3e51aa09c15f" />

Next one using the "dedup" command to remove duplicate events based on one or more fields. 

<img width="1015" height="1064" alt="image (7)" src="https://github.com/user-attachments/assets/53d6c984-5691-4ce9-aa59-6026ef154f19" />

Now using the "stats" command

<img width="2429" height="812" alt="image (8)" src="https://github.com/user-attachments/assets/3fe03e47-d99e-46ef-9968-c3bbe0c5e805" />

Another one using the "timechart" command

<img width="2432" height="856" alt="image (9)" src="https://github.com/user-attachments/assets/5002ca2b-0902-474a-81a4-99c926551e00" />

Using command "eval" 

<img width="2432" height="856" alt="image (10)" src="https://github.com/user-attachments/assets/901d6298-49da-4ed3-b1f2-09be35ec7025" />

Command "fillnull"

<img width="2432" height="856" alt="image (11)" src="https://github.com/user-attachments/assets/48af137d-d860-411f-b765-47a1a8379284" />

Command "coalesce"

<img width="2432" height="856" alt="image (12)" src="https://github.com/user-attachments/assets/82173267-ca80-42ec-a5e5-6a40f9511b0f" />












