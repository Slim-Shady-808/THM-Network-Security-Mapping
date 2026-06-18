## Overview
- Sniffing: Capture network traffic
- MITM Attacks: Intercept traffic
- Hydra Brute Force: Credential attack
- SSH/TLS: Hardening

#### Sniffing Attacks
Captures packets in network segment 

**Use Cases:**
- Capture cleartext credentials in FTP, HTTP, POP3, or IMAP
- Verify how an attacker could read your network

**Tcpdump Commands:**
```
# Capture POP3 traffic and display contents
sudo tcpdump port 110 -A

# Capture all traffic on an interface and write to file
sudo tcpdump -i eth0 -w capture.pcap

# Capture traffic on a specific port
sudo tcpdump port 23 -A      # Telnet
sudo tcpdump port 21 -A      # FTP
sudo tcpdump port 25 -A      # SMTP
```

**Wireshark Commands:**
```
# Filter to show only IMAP traffic
imap

# Filter to show only POP3 traffic
pop

# Filter to show only FTP traffic
ftp
```

#### MITM Attack
Intercepting communication between two devices

**Use Cases:**
- Understand vulnerabilities in system

**TLS Protocol Certificates**
Confirm identity of server

### SSH 
Encrypted replacement of telnet

**Use Cases:**
- Assess remote system vulnerabilities
- Sceure file transfer

**SSH Commands:**
```
# Connect to a remote system
ssh username@TARGET_IP

# Connect on a non-standard port
ssh username@TARGET_IP -p PORT

# SFTP file transfer
sftp username@TARGET_IP
```

#### Hydra 
Tool for login tests

**Use Cases:**
- Determine weak or strong credentials
- See brute-force vulnerability

**Hydra Commands:**
- -l username: provide login username
- -P WordList.txt: specify password
- -s PORT: specify port
- -V or -vV: show attempted username and password combinations
- -d: display debug output
```
hydra -l USERNAME -P WORDLIST SERVICE://TARGET_IP
hydra -l USERNAME -P WORDLIST SERVICE://TARGET_IP -V
hydra -l USERNAME -P WORDLIST -s PORT SERVICE://TARGET_IP
```

### Practical Application
**Commands:**
```
# Sniffing — capture POP3 traffic and read credentials in ASCII
sudo tcpdump port 110 -A

# Sniffing — capture all traffic and save to file for Wireshark
sudo tcpdump -i eth0 -w capture.pcap

# Hydra — test FTP login
hydra -l frank -P /usr/share/wordlists/rockyou.txt ftp://TARGET_IP -V

# Hydra — test SSH login
hydra -l frank -P /usr/share/wordlists/rockyou.txt ssh://TARGET_IP -V

# SSH — connect once valid credentials are found
ssh frank@TARGET_IP
```
