# Intelligence-Gathering
Utilizing Kali Linux 2022.2 to discover systems in a lab environment.

<h1>Scenario</h1>

A client has recently started using a new MSP (managed service provider), but they are not sure the technology is implented securely. I will conduct an assessment by performing initial reconnaissance and information gathering by reviewing the client's website, extracting information using whois and performing DNS reconnaissance - all whie retaining command history for a pentest report.

**<p style="font-size: 15px;">Step 1: Find and explore a clients website.</p>**

The below picture is a representation of the following. First, I oriented myself within the network utilizing "ip a s eth0". This displayed my ip address of 203.0.113.66. Next, I captured the output of a 4 request ping scan of the clients website, structureality.com, into a file named client_info.txt. I used the "ls -l" command to confirm that the output file existed and contained data of 178 bytes. I then uses the command, "cat client_info.txt" to display the contents of the output file. Finally, I used "sudo find / -name client_info.txt" to locate the location of the file.  

![Alt Text](https://github.com/kvweldon/Gathering-Intelligence/blob/main/Screenshot%202024-03-20%20173355.png?raw=true)

I then used Firefox to view the company's website and get an understanding of the information presented there.

![Alt Text](https://github.com/kvweldon/Gathering-Intelligence/blob/main/Screenshot%202024-03-20%20173104.png?raw=true)

**<p style="font-size: 15px;">Step 2: Whois reconnaissance.</p>**

I performed a whois query using the whois database host of 192.0.2.10 against the client's registered domain name and captured the output to a file by entering: "whois -h 192.0.2.10 structureality.com > client_whois.txt". I was then asked to identify the registrar of the domain name which is 515WEB INC.

![Alt Text](https://github.com/kvweldon/Gathering-Intelligence/blob/main/Screenshot%202024-03-20%20193106.png?raw=true)

**<p style="font-size: 15px;">Step 2: DNS reconnaissance.</p>**

![Image](https://github.com/kvweldon/Gathering-Intelligence/blob/main/Screenshot%202024-03-20%20194757.png?raw=true)
