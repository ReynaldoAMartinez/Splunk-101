# Creating Actionable Alerts

We’ll create an alert to be a brute force activity

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
```
we modify our search
```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|stats count by user
```

<img width="2441" height="933" alt="image" src="https://github.com/user-attachments/assets/e27064a5-59e7-44ee-b184-41cb0ba1a28f" />

we introduce command  bucket that is similar to time chart, where I am instructing Splunk to slice the data depending on the time span that I input here.

Split the time in five minutes intervals

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|bucket span=5m _time
|stats count by _time,user
```

<img width="2441" height="933" alt="image (1)" src="https://github.com/user-attachments/assets/b1cf2f84-c694-4abc-8bd6-f0a20d328ee2" />

Adding a counter greater than 100

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|bucket span=5m _time
|stats count by _time,user
|where count >= 100
```
<img width="2448" height="480" alt="image (2)" src="https://github.com/user-attachments/assets/9b328117-a330-46a3-bf15-b172d008bdfa" />

Adding the source IP

```bash
index=mydfir-soc source="auth.csv" sourcetype="Linux:auth" host="splunk"
|bucket span=5m _time
|stats count by _time,user,src_ip
|where count >= 100
```

<img width="2448" height="480" alt="image (3)" src="https://github.com/user-attachments/assets/b287753c-3d87-4159-8de8-8c273e9174cc" />
Now we start to create our alert

Save As / Alert and the next settings

To include Cron schedule this is the link to learn more about it Cron: https://crontab.cronhub.io/

five * * * * * = run the alert every minute

<img width="817" height="943" alt="image (4)" src="https://github.com/user-attachments/assets/e5b3ed42-8f6f-40cd-b670-1f512f3c2873" />

We see some alerts

<img width="1440" height="312" alt="image (5)" src="https://github.com/user-attachments/assets/f38961fa-e665-44a2-bb73-b75fcf15dbd6" />

<img width="2447" height="601" alt="image (6)" src="https://github.com/user-attachments/assets/21184652-0377-4551-82ee-b59f32ec2002" />

Go to Activity / Triggered alerts

<img width="2447" height="601" alt="image (7)" src="https://github.com/user-attachments/assets/aa838363-f808-4a69-aa1c-04af29bec5e1" />

we see the query and data that is responsible for this alert

<img width="2447" height="601" alt="image (8)" src="https://github.com/user-attachments/assets/bf856ab6-c135-49d9-98e2-39c0e632c47a" />

we disable the alert 

<img width="2447" height="601" alt="image (9)" src="https://github.com/user-attachments/assets/eb28ac84-70da-4662-b49a-54f3b7ae961f" />

Now we do not see more alerts because it was disabled. Once it is disabled, remove all the previous alerts from the list.

<img width="2447" height="601" alt="image (10)" src="https://github.com/user-attachments/assets/bde06e03-589c-4d1a-8ffe-ae6c05530452" />

This is the procedure how to create an alert in Splunk.

















