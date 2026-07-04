# TryHackMe — Network Security
**Write up on THM’s Network Module**
Room Description: Pentesting networks and finding vulnerabilities. Active and passive reconnaissance, Nmap, and protocols & servers.

<img width="1080" height="1080" alt="unnamed" src="https://github.com/user-attachments/assets/479db419-01dc-4ba8-842f-dbe01839b329" />

**Disclamer:**
This document contains materials and information that can be potentially damaging or dangerous. Refer to the laws in your province/country before accessing, using, or in any other way utilizing these materials. These materials are for educational and research purposes only. Persons accessing this information assume full responsibility for the use and agree not to use this content for any illegal purpose.

## Overview
The TryHackMe Network Security Module contains basic tools for pentesting your network and checking for vulnerabilities. The module covers 
- Passive reconnaissance
- Active reconnaissance
- Nmap Live Host Discovery
- Nmap Basic Port Scans
- Nmap Advanced Port Scans
- Nmap Post Port Scans
- Protocols and Servers
- Protocols and Servers 2
- Net Sec Challenge

### Room 1 — Passive Reconnaissance
The purpose of passive reconnaissance is to gather intel without direct interaction. The three command line tools are whois, nslookup, and dig. The other two web tools are DNSDumpster and Shodan.io.

The **whois tool** is a command line tool that queries domain registration databases (owner, name, dates).
```
$ whois example.com
````
**Nslookup** is a command line tool that queries DNS record servers, searching up IP addresses, mail server records, and TXT records.
```
$ nslookup -type=A example.com       # IP address(es)
$ nslookup -type=MX example.com      # mail servers
$ nslookup -type=TXT example.com     # text records
```
Similar to nslookup is the **dig tool**, as it queries the same DNS servers, but returns a more precise, detailed output. The main advantage of nslookup lies within the less detailed output, as it makes it easier to spot outliers.
```
$ dig example.com A
$ dig example.com MX
$ dig example.com TXT
```
**DNSDumpster (DNSDumpster.com)** and **Shodan (Shodan.io)** are web tools that are useful for mapping domains and their subdomains. DNSDumpster searches and pulls DNS data attached to a domain. It is useful for returning IP addresses, subdomains, and mail servers in one query. Shodan is useful for searching for open ports and software versions, as it scans for devices connected to the internet.
Example commands:
```
# WHOIS lookup on a domain
whois DOMAIN.com

# Check all DNS exposed records
dig DOMAIN.com ANY
dig @1.1.1.1 DOMAIN.com TXT

# Zone transfer attempt
dig DOMAIN.com AFXR

# Find Public IP
curl -s ifconig.me

# Check IP Exposure on Shodan
visit: https://wwww.shodan.io/host/PUBLIC_IP
```

### Room 2 — Active Reconnaissance
Active reconnaissance involves direct interaction with the target, which may result in activity appearing in the target’s logs. This type of reconnaissance should only be used with explicit authorization. The four types of active recon covered in the module are ping, traceroute, telnet, and netcat.

**Ping** is a command line tool that sends ICMP echo request packets, and listens for a reply to be returned. It is useful for quickly verifying that the host is up and the network path is open. It is important to note that firewalls do typically block ICMP requests.
```
$ ping -c 4 192.168.1.1
```
**Traceroute** is a command line tool mapping the network path between your machine and a target host. It indicates how many “hops” (routers or firewalls) are between the two machines. It also may indicate if a router or firewall is configured to block ICMP packets. 
```
$ traceroute 192.168.1.1
```
**Telnet** is a command line tool that opens a TCP connection to any specified port. It is useful for remote login sessions It is useful for banner grabbing and revealing the server software and version. Knowing the software version gives pentesters a better starting point to search for known vulnerabilities.
```
$ telnet 192.168.1.1 80
```
**Netcat** is a command line tool also useful for banner grabbing. It is similar to Telnet, but also can work with UDP connection and has a cleaner output. 
```
$ nc 192.168.1.1 80
$ nc 192.168.1.1 22
```
Example Commands: 
```
# Manual Ping Sweep of Subnet
ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i UP"; done (for i in {1..254})

# Banner Grab Servies
nc -nv 192.168.1.1 22       # SSH 
nc -nv 192.168.1.1 80       # HTTP 
telnet 192.168.1.1 23       # Telnet

# Trace Network Route to Internet
traceroute 8.8.8.8
```

### Section 3 - Nmap (Network Mapper) 

**Nmap** is the most common tool for network scanning. It can be used for host discovery, port scanning, service discovery, and scripting vulnerbaility checks. 

#### Room 3 - Host Discovery
Before beginning port scans, it is advisable to scan what hosts are on the network to see what is in your environment. 

**ARP scanning** is useful for local network scanning, as ARP requests are unblockable by the host firewall.
```
$ sudo nmap -PR -sn 192.168.1.0/24
```
**ICMP** and **TCP/UDP** ping scans are useful in reaching across networks/subnets. Using a “-sn” flag skips port scanning to retrieve results faster. 
```
$ sudo nmap -PE -sn 192.168.1.0/24      # ICMP 
$ sudo nmap -PS22,80 -sn 192.168.1.0/24 # TCP SYN to specific ports
$ sudo nmap -PU53 -sn 192.168.1.0/24    # UDP ping
```
**Port Scans** - The following port scans determine what services are online and listening. The two basic scan types are TCP Connect and TCP SYN, while Null, FIN, Xmas, ACK and Idle/Zombie Scans are more advanced scans for evading detections or gathering additional information. These are useful for more detailed network mapping, especially when SYN scans are blocked or send alerts to the target. 

#### Room 4 - Basic Port Scans 

**TCP Connect (-sT)** scans go through a full three-way handshake. While not requiring root permissions, it makes the scan more likely to be noticed by an auditor. **TCP SYN (-sS)** works by sending a SYN packet and waits for a response, which is faster and stealthier than TCP Connect, but requires root privileges. There is also a **UDP scan (-sU)**, which is slower, but required for detecting UDP-specific services.
```
$ nmap -sT 192.168.1.1       # TCP Connect
$ sudo nmap -sS 192.168.1.1  # TCP SYN
$ sudo nmap -sU 192.168.1.1  # UDP
```
The six port states are open, closed (nothing on the port), filtered (blocked by firewall), unfiltered, open|filtered, and closed|filtered.

### Room 5 - Advanced Port Scans

**Null (-sN), FIN (-sF)**, and **Xmas scans (-sX)** work through changing TCP flags to get by firewalls that only filter SYN packets. 
```
$ sudo nmap -sN 192.168.1.1   # Null
$ sudo nmap -sF 192.168.1.1   # FIN
$ sudo nmap -sX 192.168.1.1   # Xmas
```
**ACK scans (-sA)** look for unfiltered ports (not necessarily open) through the firewall ruleset. 
```
$ sudo nmap -sA 192.168.1.1
```
**Idle/Zombie Scans (-sI)** routes the network traffic into a fake host, so the target logs only show the “zombie” Ip, not your own. 
```
$ sudo nmap -sI ZOMBIE_IP 192.168.1.1
```
**Spoofing IPs** 
```
$ sudo nmap -S SPOOFED_IP 192.168.1.1
$ nmap -D DECOY1,DECOY2,ME 192.168.1.1
$ sudo nmap -f 192.168.1.1
```
### Room 6 - Post-Port Scans
Once open ports have beem identified, these scans are useful for finding the service name, service version, and OS running within the machine. 

**-sV** is a probe scan that checks each port for the service name and version.
**-O** attempts to detect the OS version.
**-A** is a scan that checks for all three, using scripts and traceroute. 
```
$ nmap -sV 192.168.1.1
$ sudo nmap -O 192.168.1.1
$ sudo nmap -A 192.168.1.1
```
**The Nmap Scripting Engine** is useful for running targeted scripts against machines and services. Then, using the -oA flag, one can save the scan output into txt, XML, and grepable. 
```
$ nmap -sC 192.168.1.1    #Default Script
$ nmap --script=vuln 192.168.1.1    # vulnerability checks
$ nmap --script=auth 192.168.1.1    # auth weakness checks
$ nmap --script=safe 192.168.1.1    # safe scripts only
$ nmap -oA scan_results 192.168.1.1
```
### Room 7 - Protocols and Servers

This part of the TryHackMe module covers common app-layer protocols, explaining how they work and what they do. By connecting using telnet, you can manipulate the 5 protocols for file transfer, plaintext transfer, and mail transfer. 

**Five Basic Protocols**
**Hypertext Transfer Protocol (HTTP)**: Text-based protocol connected to port 80 allowing one to grab the server software and version in plaintext. It also is useful for grabbing vulnerable passwords, usernames, and data. 
```
$ telnet 192.168.1.1 80
GET / HTTP/1.1
host: 192.168.1.1
```
**FTP:** Protocol for transfering files across port 21 between a client and a server. It can be exploited to grab usernames and passwords if not configured correctly. Accessible through both telnet and Nmap. 
```
$ telnet 192.168.1.1 21
USER username
PASS password
LIST
RETR filename.txt
```
**SMTP:** Protocol for communicating with mail servers across port 25. It can be manipulated to send mail without any authentication. 
```
$ telnet 192.168.1.1 25
HELO sender.example.com
MAIL FROM:<sender@example.com>
RCPT TO:<recipient@example.com>
DATA
Subject: Test message
.
QUIT
```

**POP3:** Protocol for retrieving emails from a mail server across port 110. It can be manipulated to reveal the username and password login credentials, as well download messages from the mail server.
```
$ telnet 192.168.1.1 110
USER username
PASS password
STAT
LIST
RETR 1
QUIT
```

**IMAP:** It is a protocol similar to POP3 connected via port 143. However, instead of downloading messages, it can access them from the server directly. Similar to POP3, it can be used to expose credentials and messages as plaintext.
```
$ telnet 192.168.1.1 143
c1 LOGIN username password
c2 LIST "" "*"
c3 EXAMINE INBOX
c4 FETCH 1 BODY[]
```
c5 LOGOUT

### Room 8 - Protocols and Servers Part 2
This part of the module covers attacks that may take advantage of misconfigured cleartext protocols, as well as the more secure protocols. 

**Attacks**
**A sniffing attack** is when an attacker uses these cleartext protocols to expose credentials for later use, without sending packets or making connections with the target. The module uses tcpdump, capturing network packets and the contents. 
```
$ sudo tcpdump port 110 -A    # capture POP3 traffic in ASCII
$ sudo tcpdump port 21 -A     # capture FTP credentials
$ sudo tcpdump port 25 -A     # capture SMTP traffic
$ sudo tcpdump -w capture.pcap  # save capture for later analysis
```
**A MITM Attack** occurs when the attacker listens to traffic between the client and the server, to read credentials and potentially read and alter data/files.
**Password Attacks** are exactly what they sound like. It is when attackers attempt to gain access by repeatedly using different credentials against these cleartext protocols.
The various password attacks include:
- Password Guessing: Use knowledge of target
- Dictionary Attack: Try words from a wordlist
- Brute Force: try multiple combinations
- Credential Stuffing: Using leaked password/usernames
- Password Spraying: Trying common passwords across multiple accounts
- Hybrid Attacks: Combining the above attacks
**Hydra**
  A fast password attack tool useful for guessing passwords against a network. 
```
hydra -l username -P wordlist.txt server service
```
- **-l** specifies a username
- **-P** specifies a wordlist file
- **server** = the target IP
- **service** = targeted protocol

**Common Examples:**
```
# Attack FTP
hydra -l username -P /usr/share/wordlists/rockyou.txt 192.168.1.1 ftp

# Attack SSH
hydra -l username -P /usr/share/wordlists/rockyou.txt 192.168.1.1 ssh

# Attack IMAP
hydra -l username -P /usr/share/wordlists/rockyou.txt 192.168.1.1 imap

# Attack POP3
hydra -l username -P /usr/share/wordlists/rockyou.txt 192.168.1.1 pop3

# Use a list of usernames instead of a single one
hydra -L users.txt -P passwords.txt 192.168.1.1 ssh
```
**Hydra Options:**
- -l username = single user
- -L users.txt = file containing users
- -p Password = one password to try
- -P wordlist.txt = file containing passwords
- -s PORT = specify port
- -V or -vV = verbose output
- -t n = specify number of parallel connections
- -f = stop after finding first valid credential
- -w n = wait time between connections

**Secure Protocols**
**Secure Shell (SSH)** is the encrypted version of telnet, using a cruptographic key. The module covers:
- Password authentication
- Public key authentiation
- Certificate-based authentication
```
$ ssh username@192.168.1.1                      # password auth
$ ssh -i private_key.pem username@192.168.1.1   # key-based auth
$ ssh -p 2222 username@192.168.1.1              # non-default port
```
File transfer
```
$ sftp username@192.168.1.1                          # interactive transfer
$ scp file.txt username@192.168.1.1:/home/username   # copy a file
```
**SSL/TLS** is the layer where the cleartext protocols are changed into secure protocols. The TLS handshake process is covered:
1. Client sends supported TLS versions
2. Server response with parameters/certificate
3. Client and server agree on a secret key
4. Both sides switch to encryption

There is **implicit TLS** which dedicated an encrypted port immediately upon connection and **STARTTLS** which connects the client on the normal cleartext port, then commanding it to upgrade.

**Secure Cleartext Protocols**
- HTTPS: Port 443
- SFTP: Port 22
- SMTPS: Port 465
- POP3S: Port 995
- IMAPS: Port 993
- SSH: Port 22

### Room 9 - Net Sec Challenge 
This is the final room of the module, applying all the above tools to train a beginners level of pentesting/defending a network.

### Tool Reference
- whois: Domain registration info
- nslookup: DNS record queries
- dig: Detailed DNS queries
- DNSDumpster: DNS Mapping
- Shodan: Device search
- ping: Reachability check
- traceroute: Network mapping
- telnet: Port interaction/banner grabbing
- netcat: Port connection/banner grabbing
- nmap: Network/port scanning
- tcpdump: Network packet capture
- hydra: Network password attacks
- ssh: Encrypted remote access/file transfer

[Visit THM Network Module](https://tryhackme.com/module/network-security)
