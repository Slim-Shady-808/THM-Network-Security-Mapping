# THM-Network-Security
**OVERVIEW**
Comprehensive list of basic tools and online services for passive/active reconnaissance and network mapping. 
Based off the TryHackMe Network Security Module

**Topics Covered**
Provides list of commands and uses
Passive Reconnaissance: Info gathering w/o direct interaction (WHOIS, nslookup, dig, DNSDumpster, Shodan.io)
Active Reconnaissance: Info gathering with direct interaction (Ping, Tracerroute, Telnet, Netcat)
Nmap: Network Mapping (TCP/UDP, ARP, ICMP, Basic+Advanced Port Scans)
Protocols and Servers (Common Protocols, SSH, SSL/TLS, Hydra)

**Purpose**
Learn to use basic tools and online service for network mapping and passive/active reconnaissance.
Have access to the following:
1. Shodan.Io
2. DNSDumpster.com
3. Windows, Mac, or Linux Terminal
4. Nmap
5. Hydra

**Security Considerations**
Legal and Ethical Use
⚠️ IMPORTANT: Only perform security assessments on systems you own or have explicit written permission to test.

✅ Authorized penetration testing
✅ Security research on your own infrastructure
✅ Compliance and audit assessments
❌ Unauthorized scanning is illegal and unethical
Data Sensitivity
Scan results may contain sensitive information
Store results securely with appropriate access controls
Do not commit sensitive scan data to public repositories
Use .gitignore to exclude sensitive files
Rate Limiting
Be considerate of network bandwidth and system resources
Use appropriate timing for scans (avoid aggressive scanning on production)
Respect CVE API rate limits (NVD: 5 requests/30s without API key)
