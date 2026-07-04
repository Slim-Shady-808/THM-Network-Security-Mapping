# TryHackMe-Network-Security-Assessment 
A structured and practical network-mapping guide based off the TryHackMe Network-Security module. Provides basic tools and online services for active/passive reconnassiance, Nmap scanning strategies, and common network protocols. 

**OVERVIEW**
This write-up documents the TryHackMe Network Security Module and the practicalities for network assessment :

**Topics Covered**
Provides list of commands and uses
- Passive Reconnaissance: OSINT gathering w/o direct interaction (WHOIS, nslookup, dig, DNSDumpster, Shodan.io)
- Active Reconnaissance: Direct target network probing (Ping, Tracerroute, Telnet, Netcat)
- Nmap: Network Mapping (TCP/UDP, ARP, ICMP, Basic+Advanced Port Scans)
- Protocols and Servers (Common Protocols, SSH, SSL/TLS, Hydra)

**Write-Up:** [TryHackMe Network Security — Full Write-Up](./WRITEUP.md)

**Purpose**
Learn to use basic tools and online service for network mapping and passive/active reconnaissance.
Have access to the following:
1. Shodan.Io
2. DNSDumpster.com
3. Windows, Mac, or Linux Terminal
4. Nmap
5. Hydra

## Quick Start
```git clone https://github.com/YOUR_USERNAME/thm-network-security.git
cd thm-network-security

# Browse a specific topic
cat rooms/01-Passive-Reconnaissance
cat rooms/02-Active-Reconnaissance
cat rooms/03-Nmap/README.md
cat rooms/04-Protocols-and-Servers/02-attacks-and-hardening.md

# Jump straight to the home network assessment workflow
# See: ## Practical Home Network Assessment (below)
```
## Assessment Mode
```
# Phase 1 — Find your public exposure
whois $(curl -s ifconfig.me)
dig YOUR-DOMAIN.com ANY

# Phase 2 — Discover all hosts on your LAN
sudo nmap -PR -sn 192.168.1.0/24 -oG live-hosts.txt

# Phase 3 — Full port scan and service detection
sudo nmap -sV -O -iL live-hosts.txt -T4 -oA results/phase3-services

# Phase 4 — Vulnerability checks
sudo nmap --script vuln -iL live-hosts.txt -oA results/phase4-vulns

# Phase 5 — Protocol and credential checks
nmap -p 21,23,25,80,110,143 --open 192.168.1.0/24
nmap --script ftp-anon,ssh-auth-methods,http-default-accounts 192.168.1.0/24
```

## Directory Structure
```
thm-network-security/
├── README.md                                   # This file
├── rooms/
│   ├── 01-passive-recon/
│   │   ├── README.md                           # Notes, concepts, and answers
│   │   └── commands.md                         # Key commands used
│   ├── 02-active-recon/
│   │   ├── README.md
│   │   └── commands.md
│   ├── 03-nmap/                                # All four Nmap rooms consolidated
│   │   ├── README.md                           # Unified Nmap overview
│   │   ├── 01-live-host-discovery.md
│   │   ├── 02-basic-port-scans.md
│   │   ├── 03-advanced-port-scans.md
│   │   └── 04-post-port-scans.md
│   └── 04-protocols-and-servers/               # Both Protocols rooms consolidated
│       ├── README.md
│       ├── 01-protocol-internals.md
│       └── 02-attacks-and-hardening.md
├── tools/
│   ├── nmap-cheatsheet.md
│   ├── passive-recon-tools.md
│   └── protocol-ports-reference.md
├── notes/
│   ├── scan-types-comparison.md
│   └── attack-mitigations.md
├── results/                                    # Local scan output (gitignored)
├── .gitignore
├── Contributing.md
└── LICENSE
```

# Module Rooms
## Passive Reconnaissance 
### Purpose: 
Gather info about target device w/o direct interaction

### Key Features:
- DNS and WHOIS information gathering
- Shodan for finding exposed devices, open ports, and service banners
  
### Use Cases:
- Scoping your external attack surfaces
- Identify exposed DNS records/TXT entries
- Finding what is discoverable about your public IP
- Reconnaisance on authorized targets

### Documentation: 
See rooms/01-Passive-Recon/README.md

## Active Reconnaissance 
### Purpose: 
Directly interact with target systems to map vulnerabilities within network and services

### Key Features:
- ping for reachability testing with ICMP requests
- traceroute to map network path to target
- telnet and netcat for TCP port connection and banner grabbing
- LAN host discovery without Nmap

### Use Cases:
- Map devices on network
- Identify service versions with banner grabbing
- Verify disabled Telnet on routers/IoT devices

### Documentation:
See rooms/02-Active-Recon/README.md

## Nmap
### Purpose:
Comprehensive network scanning from live host discovery

### Key Features:
- ARP, TCP SYN/ACK, ICMP, and UDP pinging techniques for host disovery
- TCP (TCP SYN) and UDP port scanning
- Null, FIN, Xmas, Maimon, ACK, Window, and Idle/Zombie stealth scans
- Nmap Scripting engine with different categories
- Normal, XML, and Greppable output formatting for reporting

### Scan Types:
- Live Host Discovery
- Basic & Advanced Port Scans
- Post Port Scans

### Use Cases: 
- Audit of network devices
- Firewall mapping with ACK scans
- Identify vulnerable credientials and service versions
- Save time-stamped scan output 

### Documentation:
See rooms/03-Nmap/README.md

## Protocols and Servers
### Purpose:
Understand common app layer protocols and explotation/hardening

### Key Features:
- Manual protocol interaction using telnet and netcat for HTTP, FTP, SMTP, POP3, and IMAP
- Packet capture and credential extraction
- Brute force and password attacks with Hydra
- TLS/HTTPS auditing
- SSH Hardening

### Attack Types:
- Sniffing: cleartext credential capture with tcpdump/wireshark
- Man-In-The-Middle: Intercept and read client-server traffic
- Brute-Force: Attacks against logins with Hydra

### Use Cases: 
- Verify no open cleartext protocols
- Test for default credentials
- Audit device SSH configuration

### Documentation:
See rooms/04-Protocols-and-Servers/README.md

## Usage Examples
### Passive Reconnaissance Examples 
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
### Active Reconnaissance Examples
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

### Nmap Examples
```
# Find live hosts on your network and save the list
sudo nmap -PR -sn 192.168.1.0/24 -oG live-hosts.txt
grep "Up" live-hosts.txt | awk '{print $2}' > hosts.txt

# SYN scan with service detection
sudo nmap -sS -sV -p- -T4 192.168.1.1 -oA results/router-full

# Run vulnerability scripts across hosts
sudo nmap --script vuln -iL hosts.txt -oA results/vuln-scan

# Check for telnet
nmap -p 23 --open 192.168.1.0/24

# Use ACK scan to test firewall
sudo nmap -sA 192.168.1.1
```

### Hydra Examples
```
# Test default credentials on router's FTP
hydra -l admin -P /usr/share/wordlists/rockyou.txt ftp://192.168.1.1

# Password Spray
hydra -L users.txt -p 'Password1!' ssh://192.168.1.1 -u -f -t 4

# Test SSH with username
hydra -l admin -P rockyou.txt ssh://192.168.1.1

# Flags to know
-V         verbose — print every attempt
-f         stop after first valid credential found
-t 4       parallel tasks (default 16; lower = quieter)
-w 30      wait 30s between retries (avoids lockout)
-s PORT    override default port
-o out.txt save found credentials to file
```
### Protocol Verification Examples
```
# Check for running cleartext protocols
nmap -p 21,23,25,80,110,143 --open 192.168.1.0/24

# Test FTP for anonymous login
nmap --script ftp-anon -p 21 192.168.1.0/24
```
## Prerequisites 
### Active/Passive Reconnaissance
- whois: Domain registration lookups
- dnsutils: DNS queries
- traceroute: Network path mapping
- netcat: Banner grabbing and TCP connections
- curl: Public IP lookup and HTTP requests
- telnet: Manual protocol interaction
#### Installation
```
sudo apt install whois dnsutils traceroute netcat-openbsd curl telnet
```

### Nmap Requirements
- nmap: Network scanner
- Root privileges
#### Installation 
```
sudo apt install nmap
# Verify NSE script database is up to date
sudo nmap --script-updatedb
```
### Protocol Assessment Requirements
- hydra: Brute-force and password spray 
- tcpdump: Packet capture and credential extraction 
- ftp: Manual FTP client 
- openssl: TLS handshake testing and certificate inspection
#### Installation
```
sudo apt install hydra tcpdump ftp openssl wireshark-cli
```

### Verify Tools
```
which nmap hydra tcpdump dig whois nc curl ftp openssl
```

## Reporting Formats
### Nmap Scan Reports
All scans are saved to produce three formats:
- Normal (.nmap): Human-readable for quick review
- XML (.xml): Machine readable/parseable
- Greppable (.gnmap): Log analysis, troubleshooting

```
# Save all formats with a datestamp
sudo nmap -A -iL hosts.txt -oA results/scan-$(date +%F)

# Extract just the open ports from a greppable file
grep "open" results/scan-2026-05-16.gnmap | awk '{print $2, $5}'
```

### Hydra Report
```
# Save found credentials to file
hydra -l admin -P rockyou.txt ssh://TARGET_IP -o results/hydra-findings.txt
```

### Tcpdump Capture Report
```
# Capture to file for offline analysis
sudo tcpdump -i eth0 -w results/capture-$(date +%F).pcap

# Extract credentials from a capture
tcpdump -r results/capture.pcap -A | grep -iE "user|pass|login|auth"
```

## Workflow Integration
Passive External Recon Workflow
Determine information publicly visile to your domain:
- Find current public IP
- Run whois to see what your public IP exposes
- Review all public DNS records
- Look for exposed tokens
- Review open ports and banners

Network Discovery Workflow
Map active devices on your local network:
- Identify subnet
- Run ARP sweep
- Extract host list and generate report
- Perform fast portscan across hosts
- Save outputs as reports for future assessments

Full Device Audit Workflow
Find services, versions, and vulnerabilities on devices:
- Run SYN scan on all ports
- Run service/OS detection
- Run NSE vulnerability scripts
- Audit SSH
- Audit web interfaces
- Save outputs as reports

Credential Testing Workflow
Test for weak/default credentials:
- Test most common default credentials
- Expand wordlist to pull txt files
- Test and search for open services
- Save output as a report

Protocol Hardening Workflow
Check that cleartext protocols are blocked and secure alternatives are configured:
- Scan subnet for cleartext services
- Confirm FTP anonymous login is disabled
- Confirm SSH authentication protocols
- Test SMTP vulnerability
- Verify HTTPS headers
- COnfirm TLS 1.0/1.1 are no longer open

## Security Considerations
### Legal and Ethical Use
⚠️ IMPORTANT: Only perform security assessments on systems you own or have explicit written permission to test.

✅ Authorized penetration testing
✅ Security research on your own infrastructure
✅ Compliance and audit assessments
❌ Unauthorized scanning is illegal and unethical
### Data Sensitivity
Scan results may contain sensitive information
Store results securely with appropriate access controls
Do not commit sensitive scan data to public repositories
Use .gitignore to exclude sensitive files
Replace real IPs and hostnames in write-ups with 192.168.1.x or TARGET_IP before pushing

### Rate Limiting
Be considerate of network bandwidth and system resources
Use appropriate timing for scans (avoid aggressive scanning on production)
Respect CVE API rate limits (NVD: 5 requests/30s without API key)
Avoid running --script vuln scans continuously — they are intrusive and may trigger alerts

## Troubleshooting
### Common Issues
#### Nmap permission denied:
```
# SYN scans, OS detection, and ARP scans require root
sudo nmap -sS TARGET
sudo nmap -O TARGET
sudo nmap -PR -sn SUBNET
```
#### Nmap returns no open ports:
```
# Skip host discovery if ICMP is blocked
nmap -Pn TARGET
nmap -Pn -p- TARGET
```

#### Tools not found
```
sudo apt install dnsutils          # dig, nslookup
sudo apt install netcat-openbsd    # nc
sudo apt install hydra             # credential testing
sudo apt install nmap              # scanning
```

#### Hydra fails immediately:
```
# Verify the service is actually running
nc -nv TARGET PORT

# Slow down to avoid connection resets
hydra -l admin -P wordlist.txt ssh://TARGET -t 4 -w 10
```

## Documentation
Passive Recon: rooms/01-passive-recon/README.md
Active Recon: rooms/02-active-recon/README.md
Nmap: rooms/03-nmap/README.md
Protocols and Servers: rooms/04-protocols-and-servers/README.md
Nmap Cheat Sheet: tools/nmap-cheatsheet.md
Passive Recon Tools: tools/passive-recon-tools.md
Protocol Port Reference: tools/protocol-ports-reference.md
Scan Types Comparison: notes/scan-types-comparison.md
Attack Mitigations: notes/attack-mitigations.md

## Contributing
Contributions are welcome! Areas for enhancement:

Additional NSE script examples with sample output
Protocol capture walkthroughs with pcap files
Device-specific hardening guides for routers, NAS devices, and printers
Corrections and clarifications to existing notes
See Contributing.md for guidelines.

## Support
For issues, questions, or contributions:

GitHub Issues: https://github.com/Slim_Shady_808/thm-network-security/issues
Documentation: See individual room READMEs in the rooms/ directory

Related Projects
This framework is part of the NetSecTap Labs ecosystem:

Network Fundamentals: Core networking concepts — OSI model, TCP/IP, subnetting
Network Security: Recon, Nmap, protocols, and attacks (this repository)
Vulnerability Research: Exploiting discovered vulnerabilities and CVE research
TryHackMe Module Page: https://tryhackme.com/module/network-security

License
This project is licensed under the MIT License. See LICENSE for details

Last Updated: May 2026

⚠️ Disclaimer:  This repository is for educational purposes and authorized security testing only. Always obtain proper authorization before scanning or assessing any network or system. Unauthorized testing may be illegal in your jurisdiction.
