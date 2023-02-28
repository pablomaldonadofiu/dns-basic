<p align="center">
<img src="https://i.imgur.com/bOTO59I.jpg" alt="Microsoft Active Directory Logo"/>
</p>

<h1>DNS (Azure)</h1>
This tutorial about DNS where we will inspect/change DNS records, and see how 2 machines interact with each other with DNS <br />



<h2>Basic Concepts</h2>

- DNS: For humans it is easier to learn a web page name than IPs, that is why we have DNS. DNS converts computer names (i.e. google.com) to IP addresses which can be used by computer to locate resources. 
- A-Records: A-record indicates the IP address of a given domain. For example, if you pull the DNS records of cloudflare.com, the A record currently returns an IP address of: 104.17.210.9.
- CNAME: The ‘canonical name’ (CNAME) record is used in lieu of an A record, when a domain or subdomain is an alias of another domain

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Steps Overview</h2>

- Inspect DNS A-records on the server
- create some of our own A-records on the server and see them from the client
- delete records from server and inspect/clear the client DNS cache 
- Basics of CNAME

<h2>Deployment and Configuration Steps</h2>


<p>
Inspect DNS A-records on the server: </p>
<p>*note: we are using the VM from the previous lab, we are connecting DC and client-1, you can use the same VM or create your own. </p>
<p>To inspect/create A records, at the DC VM, go to Server Manager and click Tools in the top right Then select DNS. Once inside DNS click on DC-1 -> Forward Lookup Zones -> mydomain.com . There you can see the A-records. Now right Click -> New Host (A or AAAA).. 
For the name type “mainframe” and for the IP address type the same IP as DC-1 (10.2.0.4 in this case) and click Done to finish
</p>
<p>
<img src="https://i.imgur.com/s2UvEu9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Changing A-Records:</p>
<p> Inside the command line of client-1 type “ping mainframe” and should return ping with no error. 
Now let’s see what happens when you change “mainframe” info, so go ahead an change mainframe ip (by right clicking it) to 8.8.8.8 
Go back to client-1 and ping mainframe. As you can see, even when we changed the IP address of mainframe, client-1 still pinging the old IP of mainframe (10.2.0.4 in this case) This is because Client-1 has its own DNS records (aka DNS CACHE) and it remembers “mainframe” as the first given IP. To check DNS cache type “ipconfig /displaydns”
</p>
<p>
<img src="https://i.imgur.com/18YRbL1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
To change DNS CACHE, let’s type “ipconfig /flushdns”, this will delete the DNS cache in client-1. You can check that the DNS CACHE was deleted by typing “ipconfig /displaydns” and check if its empty. 
Now Type mainframe and it should ping the new IP given to mainframe, this is because client-1 does not have a record of mainframe, so it asks DC-1 for the info, and DC-1 will return the new IP given. 
</p>
<p>
<img src="https://i.imgur.com/E4sSV6i.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />


<p>
working with CNAME: Let’s create a CNAME record with name “search” that points to “google.com” 
For that, go back to DC-1, go to DNS-manager right click -> New Alias (CNAME) 
</p>
<p>
<img src="https://i.imgur.com/MQMooHe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Name it “search” and for Fully qualified domain name.. type www.google.com and click OK 
Now go back to client-1 and in the command line type “ping search”, the ping should respond back by pinging google.com. So as you can see, now the domain “search” will be use the same as if you were using www.google.com
</p>
<p>
<img src="https://i.imgur.com/MAMaQ8B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
You can check DNS records and see the new CNAME record by typing “ipconfig /displaydns”
</p>
<p>
<img src="https://i.imgur.com/iHmAopr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
