# BachelorThesis

Investigation of technological vulnerabilities in web applications and databases (in bulgarian)

# Summary

Modern society relies on web technologies but with their growing popularity the risk of their use increases. 
Web applications make life easier for users and businesses, but their popularity makes them an important and 
common target for hackers who focus on their main flaw - the presence of vulnerabilities. In this thesis I 
study the way of finding and exploiting vulnerabilities, with the ultimate goal to take measures to maintain 
protection.

The percentage of cybercrime is increasing with each passing day and that is why it is very important to know 
the current statistics and trends in cybersecurity. Numerous reports show that vulnerabilities in web applications 
continue to be the main cause of security and data breaches. Within the thesis you can find some of the most risky 
data breaches in the 21st century, many of which were possible by gaps in web applications. For example:

* Yahoo! - Cookie hijacking (3 bilion of users);
* Adult Friend Finder - Local File Inclusion vulnerability (412,2 milion accounts);
* Heartland Payment Systems - SQL Injection (134 milion credit cards);
* Anthem - Phishing link (78,8 milion users);
* RSA Security - Series of phishing attacks (40 million records of employees);
* Bulgarian National Revenue Agency - Vulnerability of one of the e-services for VAT refund 
                                      (57 folders from a total of over 110 compromised databases, 21 GB of data);

The process of conducting complex cyberattacks can be described as the life cycle of an attack, which consists 
of several steps. Following a methodology is not mandatory, but gives a better and more effective results. Attack
lifecycle is a explained in details in a separate chapter of the thesis. The practical part  covers the phases 
Reconnaissance, Scanning and Exploitation. The other phases are not in the scope of this thesis. 

Before continuing with a brief overview of the three attacks, thoroughly explained in the thesis, let's clarify 
few concepts:

* Vulnerability - a technological flaw that allows hackers to experiment and possibly successfully compromise the 
security of a target system;

* Exploit - program code, part of software, a piece of data or a sequence of commands that allows a malicious person 
to take advantage of a technological vulnerability;

* Payload - the modules that are launched on the victim system if an exploit has been performed successfully;

## XSS-Attack

![alt text](https://github.com/DesislavaPA/BachelorThesis/blob/master/attacks-overview/xss-attack-overview.JPG?raw=true)

Phase 1: This is the initial network topology. The attack is is carried out within the local network 192.168.0.0/24. There 
are two target systems - one is an OWASP Broken Web Application virtual machine, it acts as a web server, the other is a 
Windows 10 host OS, or it is the victim whose browser will intercept. The IPv4 address of the web server is 192.168.0.103 
and that of the victim is 192.168.0.101. The attacker system is another virtual machine that has Kali Linux installed with 
192.168.0.106 IPv4 address. The network cards of both virtual machines are configured as a "Bridged Adapter", in which the
virtual machine connects to the network using an Ethernet adapter on the host computer and directly exchanges network packets, 
surrounding the network stack of the host operating system. This is how we simulate physically separate machines in the network.

Phase 2: The next step is the "ARP Cache Poisoning" attack - one of the oldest MITM (Man in the middle) approaches. ARP spoofing 
is a technique by which an attacker sends spoofed Address Resolution Protocol (ARP) messages to a local network. The purpose is 
to associate the attacker's MAC address with the IP address of another host, which means that any traffic destined for that 
IP address is sent to the attacker. ARP spoofing can allow a hacker to intercept data frames on a network, change or stop all traffic.

Phase 3: After the successful MITM attack (phase 2), we will modify the traffic from the server to the client using a filter. 
Add hook.js to the server response (step 1). As soon as the client receives the response and the hook is loaded in 
his browser, the BeEF communication server will start communicating with the victim and we will see the intercepted browser 
on the BeEF user interface (step 3). Then we will be able to send various commands and attacks (you can see results in the power 
point presentation slide 6).

## Sql-Injection

Phase 1: The topology is similar to the previous one with the difference that the web server has an IPv4 address 192.168.0.109 
and the target web application is WackoPicko.

![alt text](https://github.com/DesislavaPA/BachelorThesis/blob/master/attacks-overview/sql-injection-attack-overview.JPG?raw=true)

Phase 2: The exploit consists of the following. First, a php payload named bd.php is generated with msfvenom; Then, using sqlmap 
(3 and 4) upload the payload to the web server exploiting the SQL Injection Vulnerability discovered in the WackoPicko application 
during the scanning phase; sqlmap supports downloading and uploading a file from the database server;

![alt text](https://github.com/DesislavaPA/BachelorThesis/blob/master/attacks-overview/sql-injection-result.JPG?raw=true)

Phase 3: After the successful exploit it is necessary for the attacking system to run a handler to listen on the port specified 
in bd.php (4444) - Step 2. In Step 3 we are starting the script 'http://192.168.0.109/WackoPicko/users/bd.php' in a browser that 
is programmed to establish a TCP connection to the attacking machine on port 4444. If step 3 completes successfully it will establish 
a session between the attacking machine and the web server and the meterpreter shell will open on the web server - Step 4. I will leave 
it up to your imagination what could happen next if the attacker was a malicious hacker - just kidding post-exploitation and maintaining 
access in case of established backdoor (method for bypassing normal authentication to access a computer system) is explained in 5.2.5 
subsection.

## Database server attack

Phase 1: The topology is similar to the previous one. The web server has an IPv4 address of 192.168.0.103. For the purposes 
of the example, it will be necessary for the database server to be accessible by all participants in the network.

![alt text](https://github.com/DesislavaPA/BachelorThesis/blob/master/attacks-overview/xss-attack-overview.JPG?raw=true)

Phase 2: Step 1 is an attack using a pre-prepared dictionary file and username. The dictionary file is a list of common 
passwords. We can achieve this type of attack using Metasploit and the mysql_login module. The attacked user is the standard 
user with administrative rights in MySQL - root. The attack will only be successful if the dictionary file contains the password 
used by the root user (in this case ‘pandora’). Once we have found the password used by the user, we try to access the MySQL server 
(Step 2). Slide 10 of the power point presentation shows result of deleting one of the databases after gaining access to the server.
