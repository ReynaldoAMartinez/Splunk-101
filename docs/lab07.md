# Dashboard Creation

We will create three dashboards using the authentication log file

Set a time range —> from Jan 7 2024 to Jan 8 2024

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
```

<img width="2412" height="1003" alt="image" src="https://github.com/user-attachments/assets/bd63d194-8c03-4dbf-8714-a4fb31d5853f" />

First dashboard —> single value dashboard —> show a user that accumulate the most failed login

Use a transformation command

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|stats count by user
```

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" failed
|stats count by user
```

<img width="2451" height="839" alt="image (1)" src="https://github.com/user-attachments/assets/364bcd4d-f0c1-4a80-b121-1e312e9d2eb0" />


```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" failed
|stats count by user 
|sort -count
```

<img width="2451" height="839" alt="image (2)" src="https://github.com/user-attachments/assets/36998389-24b4-4a8b-85cf-ed572d43fd6d" />

root is the top user with failed login, so to create a dashboard 

Save As / New Dashboard

<img width="605" height="743" alt="image (3)" src="https://github.com/user-attachments/assets/7540e7ed-a626-4901-a829-b846323f2622" />

<img width="2431" height="694" alt="image (4)" src="https://github.com/user-attachments/assets/26a1739d-2479-4204-b2ce-b8394bcea101" />

to change the table in a different dashboard —> Edit —> Change Chart Visualization to Single Value

<img width="2431" height="694" alt="image (5)" src="https://github.com/user-attachments/assets/5a87dcc4-4e6d-4105-8edd-797f15a8181a" />

Next panel is a time based aka time chart panel 
```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" failed
|timechart span=1h count by user
```
<img width="2424" height="983" alt="image (6)" src="https://github.com/user-attachments/assets/a131e15a-71a7-4a2b-9fb4-1e1e221fb5ec" />

Save As or Visualization and change the Chart

<img width="2424" height="983" alt="image (7)" src="https://github.com/user-attachments/assets/ecbf6049-41ff-49dc-9351-6ca7a8a43685" />

Save As / Existing Dashboard —> select the Linux Activity

<img width="569" height="509" alt="image (8)" src="https://github.com/user-attachments/assets/75903443-a396-4550-9ef3-c54466eb75d8" />

<img width="2439" height="525" alt="image (9)" src="https://github.com/user-attachments/assets/09e299a6-052d-4b56-a26b-cc8da6e30613" />

Here we can ask …were there successful logins during this pike ? Next dashboard will answer that question

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" accepted
|stats count by user
```

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" accepted
|stats count by _time,user,src_ip
|sort -_time
```
<img width="2439" height="525" alt="image (10)" src="https://github.com/user-attachments/assets/639824d2-2420-4c83-8c37-504ef90f8548" />

Save As / Existing Dashboards —> Successful Logins

<img width="2448" height="784" alt="image (11)" src="https://github.com/user-attachments/assets/2637283f-3475-497a-9913-758e517a4108" />
































