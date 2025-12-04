
Go to www.splunk.com to download the Splunk Universal Forwarder 

<img width="1548" height="773" alt="image" src="https://github.com/user-attachments/assets/e7d581e1-20a6-4c5c-9719-a0e7ff808f74" />

<img width="523" height="412" alt="image (1)" src="https://github.com/user-attachments/assets/f5793db6-36b4-4fd9-b25c-d0de4885f256" />

<img width="502" height="394" alt="image (2)" src="https://github.com/user-attachments/assets/f54b9b44-84a7-4664-a2fc-4afd49bd69c8" />

<img width="516" height="411" alt="image (3)" src="https://github.com/user-attachments/assets/a0ac53af-5273-4abe-8c15-125bd19c163e" />


Access Splunk / Settings / Forwarding and receiving
<img width="617" height="631" alt="image (4)" src="https://github.com/user-attachments/assets/c3ada703-57a1-428e-8d16-a98829559675" />

<img width="1106" height="442" alt="image (5)" src="https://github.com/user-attachments/assets/91645dd7-107b-40d8-a259-aa16a87d5e7c" />


Configure receiving

New Receiving Port
<img width="1091" height="348" alt="image (6)" src="https://github.com/user-attachments/assets/ac4c7552-37ed-42f4-8b1f-0036ef270bdd" />

<img width="1163" height="664" alt="image (7)" src="https://github.com/user-attachments/assets/519f376a-e7ae-4579-8392-a3266aaf2188" />



under etc / system / default   is the configuration files used by Splunk

if Splunk cannot find the file in local, it search it in default

we should edit  inputs.conf

<img width="265" height="173" alt="image" src="https://github.com/user-attachments/assets/e6eeb767-2ffa-41b8-92b1-feb0436b3532" />

restart our service
<img width="1333" height="727" alt="image (8)" src="https://github.com/user-attachments/assets/c7cae316-043d-4028-96fb-daf66e139c0c" />


<img width="427" height="479" alt="image (9)" src="https://github.com/user-attachments/assets/e8b01c53-fbd9-4642-85e6-7bd785bd067e" />


Restart the service

Go to Splunk

search index=mydfir-soc
<img width="918" height="646" alt="image (11)" src="https://github.com/user-attachments/assets/f5f852e5-8036-487c-bbd4-4f1c8414f501" />

we can see the VM and in the source WinEventLogSecurity
<img width="918" height="646" alt="image (12)" src="https://github.com/user-attachments/assets/c4d17dad-f449-4dbe-bc93-c165c4d8354e" />


If I want to ingest Applications, I should edit the file again with the next lines

<img width="225" height="188" alt="image (13)" src="https://github.com/user-attachments/assets/bed8f589-5efb-4c0d-9b92-9cdd9a03e1d2" />

restart the SplunkForwarded service to update the ingestion to Splunk

<img width="597" height="274" alt="image (14)" src="https://github.com/user-attachments/assets/3471e344-dbdc-481e-b228-b9b104356380" />



