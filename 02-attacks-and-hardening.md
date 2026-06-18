## Overview
- Sniffing: Capture network traffic
- MITM Attacks: Intercept traffic
- Hydra Brute Force: Credential attack
- SSH: Hardening

#### Sniffing Attacks
Captures packets in network segment 

**Use Cases:**
- Capture cleartext credentials in FTP, HTTP, POP3, or IMAP
- Verify how an attacker could read your network

**Tcpdump Commands**
```
# Capture all traffic on interface and save to file
sudo tcpdump -i eth0 -w capture.pcap

# Capture on a specific interface
sudo tcpdump -i eth0 -c 100 -w capture.pcap

# Capture traffic on a specific port
sudo tcpdump -i eth0 port 21 -w ftp-capture.pcap
sudo tcpdump -i eth0 port 110 -w pop3-capture.pcap

# Capture traffic to or from a host
sudo tcpdump -i eth0 host TARGET_IP -w host-capture.pcap

# Capture without saving 
sudo tcpdump -i eth0 -A port 21

# Read a capture file 
tcpdump -r capture.pcap -A

# Search for credential keywords 
tcpdump -r capture.pcap -A | grep -iE "user|pass|login|auth|password"

# Show only FTP commands
tcpdump -r capture.pcap -A port 21 | grep -iE "user|pass|230|331"

# Show HTTP headers
tcpdump -r capture.pcap -A port 80 | grep -iE "host:|authorization:|cookie:|set-cookie:"
