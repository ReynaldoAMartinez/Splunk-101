# Uploading Custom Logs

Download the zip file

Unzip to see 8 csv files

<img width="191" height="478" alt="image" src="https://github.com/user-attachments/assets/0670da7c-0f5b-4a9c-b36e-dc00627fff00" />

Splunk / Settings / Add Data

<img width="614" height="638" alt="image (1)" src="https://github.com/user-attachments/assets/5de8ea3a-f95d-4c75-97ac-da9d69f2befd" />

Choose  Upload 

max file size is 500 Mb

load Applications

click on Next

<img width="459" height="288" alt="image (2)" src="https://github.com/user-attachments/assets/801c06eb-dd3c-49b8-ad79-1990c3087a33" />

it is a warning triangle saying that Failed to parse timestamp. Defaulting to file modtime. 

it means we should say what is the column from where Splunk will take the right time.

<img width="1600" height="552" alt="image (3)" src="https://github.com/user-attachments/assets/fac593f1-aeb4-4f35-addc-9cffc2a77108" />

if we move to the right we find the right time is in the column “time”

how we say Splunk that it is the right field for time?

expand Timestamp on the left panel

Extraction is Auto , change it to Advanced

in Timestamp fields —> time    and hit ENTER

now the warning is no longer there

change time zone to (GMT)

<img width="1405" height="757" alt="image (4)" src="https://github.com/user-attachments/assets/bb186651-99c5-483b-9ef4-0d3666e6b827" />

click on Save As

Name—> WinEvent:Application

category —> custom

App —> Search & Reporting

click on Save, click on Next

<img width="891" height="591" alt="image (5)" src="https://github.com/user-attachments/assets/0a0193fc-1ef3-446b-9167-488df4b20772" />

In Input Settings

back to the first window, and copy the value of the colum   extracted_host and click Next  and under the field  Host field value —> paste the value copied

Index —> mydfir-soc

clcik on Review

Submit

<img width="874" height="262" alt="image (6)" src="https://github.com/user-attachments/assets/2b18f6d2-549b-4577-96ff-e3ecd80a4154" />

<img width="858" height="479" alt="image (7)" src="https://github.com/user-attachments/assets/148de37b-c1d4-46c5-98d6-f40215d5a406" />

Now we should see 42 events under the Application log

```bash
source="application.csv" host="FRONTDESK-PC1" index="mydfir-soc" sourcetype="WinEvent:Application"
```

<img width="1519" height="587" alt="image (8)" src="https://github.com/user-attachments/assets/9e212f46-b164-465b-a1f0-889a2484353d" />


















