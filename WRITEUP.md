#TryHackMe — Network Security
Write up on THM’s Network Module
Room Description: Pentesting networks and finding vulnerabilities. Active and passive reconnaissance, Nmap, and protocols & servers.


Disclamer:
This document contains materials and information that can be potentially damaging or dangerous. Refer to the laws in your province/country before accessing, using, or in any other way utilizing these materials. These materials are for educational and research purposes only. Persons accessing this information assume full responsibility for the use and agree not to use this content for any illegal purpose.
The TryHackMe Network Security Module contains basic tools for pentesting your network and checking for vulnerabilities. The module covers passive reconnaissance, active reconnaissance, Nmap, and common protocols and servers.
## Room 1 — Passive Reconnaissance
The purpose of passive reconnaissance is to gather intel without direct interaction. The three command line tools are whois, nslookup, and dig. The other two web tools are DNSDumpster and Shodan.io.
The whois tool is a command line tool that queries domain registration databases (owner, name, dates).
$ whois example.com
Nslookup is a command line tool that queries DNS record servers, searching up IP addresses, mail server records, and TXT records.
$ nslookup -type=A example.com       # IP address(es)
$ nslookup -type=MX example.com      # mail servers
$ nslookup -type=TXT example.com     # text records
Similar to nslookup is the dig tool, as it queries the same DNS servers, but returns a more precise, detailed output. The main advantage of nslookup lies within the less detailed output, as it makes it easier to spot outliers.
$ dig example.com A
$ dig example.com MX
$ dig example.com TXT
DNSDumpster (DNSDumpster.com) and Shodan (Shodan.io) are web tools that are useful for mapping domains and their subdomains. DNSDumpster searches and pulls DNS data attached to a domain. It is useful for returning IP addresses, subdomains, and mail servers in one query. Shodan is useful for searching for open ports and software versions, as it scans for devices connected to the internet.
Example commands:
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
## Room 2 — Active Reconnaissance
Active reconnaissance involves direct interaction with the target, which may result in activity appearing in the target’s logs. This type of reconnaissance should only be used with explicit authorization. The four types of active recon covered in the module are ping, traceroute, telnet, and netcat.
Ping is a command line tool that sends ICMP echo request packets, and listens for a reply to be returned. It is useful for quickly verifying that the host is up and the network path is open. It is important to note that firewalls do typically block ICMP requests.
$ ping -c 4 192.168.1.1

Traceroute is a command line tool mapping the network path between your machine and a target host. It indicates how many “hops” (routers or firewalls) are between the two machines. It also may indicate if a router or firewall is configured to block ICMP packets. 

$ traceroute 192.168.1.1

Telnet is a command line tool that opens a TCP connection to any specified port. It is useful for remote login sessions It is useful for banner grabbing and revealing the server software and version. Knowing the software version gives pentesters a better starting point to search for known vulnerabilities.

$ telnet 192.168.1.1 80

Netcat is a command line tool also useful for banner grabbing. It is similar to Telnet, but also can work with UDP conneciton. 

$ nc 192.168.1.1 80

Example Commands: 

# Manual Ping Sweep of Subnet
ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i UP"; done (for i in {1..254})

# Banner Grab Servies
nc -nv 192.168.1.1 22       # SSH 
nc -nv 192.168.1.1 80       # HTTP 
telnet 192.168.1.1 23       # Telnet

# Trace Network Route to Internet
traceroute 8.8.8.8

## Room 3 - Nmap (Network Mapper)

Nmap is the most common tool for network scanning. It can be used for host discovery, port scanning, service discovery, and scripting vulnerbaility checks. 

Host Discovery - Before beginning port scans, it is advisable to scan what hosts are on the network to see what is in your environment. 

ARP scanning is useful for local network scanning, as ARP requests are unblockable by the host firewall.

$ sudo nmap -PR -sn 192.168.1.0/24

ICMP and TCP/UDP ping scans are useful in reaching across networks/subnets. Using a “-sn” flag skips port scanning to retrieve results faster. 
 
$ sudo nmap -PE -sn 192.168.1.0/24      # ICMP 
$ sudo nmap -PS22,80 -sn 192.168.1.0/24 # TCP SYN to specific ports
$ sudo nmap -PU53 -sn 192.168.1.0/24    # UDP ping

Port Scans - The following port scans determine what services are online and listening. The two basic scan types are TCP Connect and TCP SYN, while Null, FIN, Xmas, ACK and Idle/Zombie Scans are more advanced scans for evading detections or gathering additional information. These are useful for more detailed network mapping, especially when SYN scans are blocked or send alerts to the target. 

Basic Port Scans: 

TCP Connect (-sT) scans go through a full three-way handshake. While not requiring root permissions, it makes the scan more likely to be noticed by an auditor. TCP SYN (-sS) works by sending a SYN packet and waits for a response, which is faster and stealthier than TCP Connect, but requires root privileges. There is also a UDP scan, which is slower, but required for detecting UDP-specific services.

$ nmap -sT 192.168.1.1       # TCP Connect
$ sudo nmap -sS 192.168.1.1  # TCP SYN
$ sudo nmap -sU 192.168.1.1  # UDP

Advanced Port Scans: 

Null (-sN), FIN (-sF), and Xmas scans (-sX) work through changing TCP flags to get by firewalls that only filter SYN packets. 

$ sudo nmap -sN 192.168.1.1   # Null
$ sudo nmap -sF 192.168.1.1   # FIN
$ sudo nmap -sX 192.168.1.1   # Xmas

ACK scans (-sA) look for unfiltered ports (not necessarily open) through the firewall ruleset. 

$ sudo nmap -sA 192.168.1.1

Idle/Zombie Scans (-sI) routes the network traffic into a fake host, so the target logs only show the “zombie” Ip, not your own. 

$ sudo nmap -sI ZOMBIE_IP 192.168.1.1

Post-Port Scans - Once open ports have beem identified, these scans are useful for finding the service name, service version, and OS running within the machine. 

-sV is a probe scan that checks each port for the service name and version.
-O attempts to detect the OS version.
-A is a scan that checks for all three, using scripts and traceroute. 

$ nmap -sV 192.168.1.1
$ sudo nmap -O 192.168.1.1
$ sudo nmap -A 192.168.1.1

The Nmap Scripting Engine is useful for running targeted scripts against machines and services. Then, using the -oA flag, one can save the scan output into txt, XML, and grepable. 

$ nmap -sC 192.168.1.1    #Default Script
$ nmap -oA scan_results 192.168.1.1



## Room 4 - Protocols and Servers

This part of the TryHackMe module covers common app-layer protocols, explaining how they work and what they do. By connecting using telnet, you can manipulate the 5 protocols for file transfer, plaintext transfer, and mail transfer. 

Five Basic Protocols
Hypertext Transfer Protocol (HTTP): Text-based protocol connected to port 80 allowing one to grab the server software and version in plaintext. 
$ telnet 192.168.1.1 80
GET / HTTP/1.1
host: 192.168.1.1
FTP
SMTP
POP3
IMAP
