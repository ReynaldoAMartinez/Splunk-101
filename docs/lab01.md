# Lab: Creating an Index
Splunk is managed by two main things: Index and Events

What I learned is before Splunk can store or search any data, it needs a place to put it in. And that is what an index is.

Index = a dedicate folder separate by categories ( windows log, system logs, firewall logs, cloud logs, etc. )

When data comes in, Splunk will write it to the index that you specify.
When I run a search, Splunk only looks inside the index that I am asking for. That is why it is common practice to create multiple indexes, as it keeps searches faster and also makes access control easier.

# Steps to create an index

Settings / Indexes

<img width="588" height="440" alt="image" src="https://github.com/user-attachments/assets/e7f6181b-5173-49d3-a346-17078f177145" />

By default I see some indexes already created. Click on the "New index" button.

<img width="2440" height="770" alt="image (1)" src="https://github.com/user-attachments/assets/2c99a228-f002-4933-993c-833fc12ade16" />

I called my index "mydifr-soc"

<img width="798" height="919" alt="image (2)" src="https://github.com/user-attachments/assets/8ded9b17-5e80-450d-9141-9fc361928aee" />

Now the index "mydfir-soc" is created

<img width="2454" height="810" alt="image (3)" src="https://github.com/user-attachments/assets/bb7fc945-6816-47b8-88bd-d047a040190b" />

Three columns suggested to troubleshooting are Event Count, Earliest Event, and Latest Event. 

