# Field Extractions

Upload the csv file to Splunk

<img width="1902" height="534" alt="image" src="https://github.com/user-attachments/assets/7b1ecee8-acfa-471c-b0de-1196fd942961" />

<img width="589" height="389" alt="image (1)" src="https://github.com/user-attachments/assets/957d8eeb-98c4-40f8-976c-754a2b5a665d" />

<img width="1077" height="777" alt="image (2)" src="https://github.com/user-attachments/assets/5a96d3dd-3f20-46d4-98ae-6c194f980ea6" />

<img width="938" height="775" alt="image (3)" src="https://github.com/user-attachments/assets/ea85e02f-3a71-4658-8190-44670ce65889" />

change the search to find Failed password that it is when there is a brute force attacks

```bash
source="auth.csv" host="splunk" index="mydfir-soc" sourcetype="Linux:auth" Failed password
```

<img width="2434" height="1271" alt="image (4)" src="https://github.com/user-attachments/assets/df6d6465-9f90-4cb4-a25f-9b320fa834c2" />

Clicked on Extract new fields to see the IP and user

Clicked in the first record and Next and we will see two options
<img width="2434" height="1271" alt="image (5)" src="https://github.com/user-attachments/assets/cbaeb4cc-9378-4b28-b3bf-5b454e2b289d" />

select regular Expresion, Next

then select each field we are interested to add with a name, and Add extraction.

we’ll see the fields added
<img width="2434" height="1271" alt="image (6)" src="https://github.com/user-attachments/assets/91cf7e0d-2340-4610-8d87-58c581b82343" />

clicked on Matches

<img width="2434" height="1271" alt="image (7)" src="https://github.com/user-attachments/assets/4a1832e1-faaa-4100-b6a2-c30f04990727" />

clicked on Non-matches

<img width="2434" height="1271" alt="image (8)" src="https://github.com/user-attachments/assets/c4d85f58-93f3-4a11-8153-e08af157d1fe" />

Next, Next, and keep the next settings

<img width="1057" height="606" alt="image (9)" src="https://github.com/user-attachments/assets/097e43fc-ed46-42c0-92d1-fc7ede55e3e3" />

Finish, select the Explore the fields I just created in Search

<img width="1969" height="1005" alt="image (10)" src="https://github.com/user-attachments/assets/352d5c50-2c86-4a5d-8c83-9e3aab31c022" />

Now we see the fields added


<img width="286" height="670" alt="image (11)" src="https://github.com/user-attachments/assets/95c71835-5824-4021-87b0-37e11457d5c2" />


<img width="1910" height="1005" alt="image (12)" src="https://github.com/user-attachments/assets/af1e9113-496e-4280-8bc6-ffd9e92732d5" />

extract new files again, repeat the steps and choose the user in the first record to create a new field

user, and  src_ip

before click on Next, click on Non-matches, we won’t see records, it is ok, now Next , Finish


<img width="1910" height="1005" alt="image (13)" src="https://github.com/user-attachments/assets/a19189e2-0be6-4831-ae90-f456fb11f179" />

After finish we’ll see this error, just change the extraction name to invalid-user,src_ip

<img width="1017" height="611" alt="image (14)" src="https://github.com/user-attachments/assets/bb325a42-c822-49f0-9123-6519ee6e056e" />

```bash
index="mydfir-soc" sourcetype=Linux:auth Failed password invalid
```

<img width="1028" height="1027" alt="image (15)" src="https://github.com/user-attachments/assets/c4069da9-95f2-4f4a-bc4e-e06ad9769731" />

```bash
index="mydfir-soc" sourcetype=Linux:auth Failed password
```
<img width="1028" height="1027" alt="image (16)" src="https://github.com/user-attachments/assets/54aba6b4-4d90-4ed0-bcd7-e1bfd0c16def" />

```bash
index="mydfir-soc" sourcetype=Linux:auth accepted
```

<img width="2068" height="1017" alt="image (17)" src="https://github.com/user-attachments/assets/0577bb23-dab9-42d7-b84c-e2c1cdf18cc4" />

Now we can see there 2 ip successfully signed as an user “root”. 



