# Scheduling Reports

Generated some data via SPL queries.
```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|stats count by _time,user,src_ip
|sort -count
```

<img width="2447" height="601" alt="image" src="https://github.com/user-attachments/assets/15b238d9-b472-4c3f-9cb2-c9df5a60ac51" />


```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" failed
|stats count by _time,user,src_ip
|sort -count
```
I could also use timechart to show daily failed logins.

<img width="2447" height="601" alt="image (1)" src="https://github.com/user-attachments/assets/281a967a-0d9d-4215-a7d3-88a978fac224" />

By removing the time, I can see the data ordered in desc

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk" failed
|stats count by user,src_ip
|sort -count
```
<img width="2438" height="667" alt="image (2)" src="https://github.com/user-attachments/assets/eba2a807-72b9-4cd3-9927-a69b3f18314e" />

the IP 178.128.225.222 was used by root account and failed 140 times

Now we create Schedule Report

Save As / Report

NOTE —> sometimes no all should be an alert, so we can create a report, to avoid too much noise with many alerts. I can instruc Splunk to run a report on Sunday to see how many failed logins exist

On Monday, I can review this report to check what is reporting.

<img width="558" height="419" alt="image (3)" src="https://github.com/user-attachments/assets/f5accd85-128f-46b4-a700-4619c7391927" />

then click on Schedule

<img width="550" height="352" alt="image (4)" src="https://github.com/user-attachments/assets/29de0e16-c14c-49d5-9493-6db47e7f598b" />


<img width="795" height="565" alt="image (5)" src="https://github.com/user-attachments/assets/5637d888-488c-455c-82d4-8cfcdba14f90" />

I created a schedule report that will run every week looking for failed logins. This report will run on Monday at midnight.


<img width="1735" height="393" alt="image (6)" src="https://github.com/user-attachments/assets/b38cfe57-0f4d-4279-b809-e573fa8cec23" />






















