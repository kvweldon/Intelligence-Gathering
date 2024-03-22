# Intelligence-Gathering
Utilizing Kali Linux 2022.2 to discover systems in a lab environment.

<h1>Scenario</h1>

A client has recently started using a new MSP (managed service provider), but they are not sure the technology is implented securely. I will conduct an assessment by performing initial reconnaissance and information gathering by reviewing the client's website, extracting information using whois and performing DNS reconnaissance - all whie retaining command history for a pentest report.

**<p style="font-size: 15px;">Step 1: Find and explore a clients website.</p>**

The below picture is a representation of the following. First, I oriented myself within the network utilizing "ip a s eth0". This displayed my ip address of 203.0.113.66. Next, I captured the output of a 4 request ping scan of the clients website, structureality.com, into a file named client_info.txt. I used the "ls -l" command to confirm that the output file existed and contained data of 178 bytes. I then uses the command, "cat client_info.txt" to display the contents of the output file. Finally, I used "sudo find / -name client_info.txt" to locate the location of the file.  

![Screenshot 2024-03-20 173355](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/b509540d-2537-4624-84f2-b8cda3c7f682)


I then used Firefox to view the company's website and get an understanding of the information presented there.

![Screenshot 2024-03-20 173104](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/5a412da6-d4c6-4ff2-ae28-207f9bb49c30)


**<p style="font-size: 15px;">Step 2: Whois reconnaissance.</p>**

I performed a whois query using the whois database host of 192.0.2.10 against the client's registered domain name and captured the output to a file by entering: "whois -h 192.0.2.10 structureality.com > client_whois.txt". I was then asked to identify the registrar of the domain name which is 515WEB INC.

![Screenshot 2024-03-20 193106](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/b9e52b90-4c84-40c6-8925-00389bb6d73f)


**<p style="font-size: 15px;">Step 3: DNS reconnaissance with nslookup.</p>**

The following pictures show how I was able to enumerate information from DNS about the client.
I first checked the lookup server being used by nslookup which is shown to be 203.0.113.226. I entered the clients FQDN to view the address resource records which resulted in the IPv4 address of 203.0.113.1 which is the address originally discovered earlier in the ping scan. The result of non-authoritative indicates that results are being returning by a caching DNS server and not directly from the authoritative server, the server that holds the actual DNS records. In the whois scan from earlier I discovered that the name server for the client, ns.structureality.com, and entered it into nslookup to resolve the name server into its IP adress, 203.0.113.225. Next, I changed the lookup server for nslookup to the IP address of the client's name server by entering the command, "server 203.0.113.225". I viewed the clients FDQN resource records a second time to confirm future results will come from an authoritative DNS server. From here, I viewed the authoritative DNS servers related to the registered domain name using the command "set type=ns" followed by the client's domain which showed the nameserver and IP address. I then entered the command "set type=mx" followed by the client's domain to display the SMTP email servers related to the registered domain name, mail.structureality.com.

![Screenshot 2024-03-20 194757](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/156318d2-b83a-414e-8b37-0dae991d1b32)

My next task was to discover the IP address of the authoritative DNS servers related to the mail server. I entered the command "set type=a" followed by the email server domain name, this resulted in an IP address of 203.0.113.1. Finally, I queried the SOA, Start of Authority, record of the registered domain name to view the origin, responsible party email, serial number and values for secondary DNS update checks. 
![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/07c22dc6-c618-4d26-8f11-1f83b008fa18)

Quering the DNS records are vital in a pentest because it gives a broader understanding of the DNS configuration, helps to identify attack vectors if there are DNS misconfigurations or outdated DNS servers, enumerates subdomains specifically in the SOA record which shows the domain admin email address, and provides insight into the redundancy and failover mechanisms for the DNS infrastructure. 

**<p style="font-size: 15px;">Step 4: DNS reconnaissance with dig.</p>**

The dig utility was used to exrtact DNS information from an authoritative DNS server registered to the client's domain and captured in a file named client_dns.txt. The command used was "dig @203.0.113.225 structureality.com > client_dns.txt". I then viewed the captured output by entering "cat client_dns.txt". This captured the SOA record for structureality.com.

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/9daedb38-60ab-4b48-9d62-76f65d09086c)

Displayed in the two pictures below is the captured output of the FDQN appended to the same client_dns.txt file. The output was then viewed, using the same cat command, which shows the same A record result from nslookup. 

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/e8303f00-4a71-4bcf-ab0e-62077c95a7f2)

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/4512a6ed-4bf1-4923-baff-5c4a16bb2b69)

Below shows me capturing the dig output from the client's domain for the resource records type MX and NS and then viewing the captured output. As with the previous task the captures were appended to the file by using ">>" in the command.

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/04463028-8d25-4a85-84d2-a53ee9f053c6)

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/3a1356d8-c9c6-4d24-ba84-325975edf549)

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/e5e60e2b-c31a-402e-b527-e4bb78fc2b8f)

**<p style="font-size: 15px;">Step 5: Exporting the command history for the pentest report.</p>**

I used the command "history > pentest_command.txt" to export the Terminal window command history and capture it in a file. I then viewed the file using the cat command. 

![image](https://github.com/kvweldon/Intelligence-Gathering/assets/141193154/897f47fa-fd56-47cd-9a29-97600be8f76e)


