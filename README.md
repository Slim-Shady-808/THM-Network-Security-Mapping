# THM-Network-Security-Passive-Reconnaissance
Based off the TryHackMe Network Security Module

## Overview
- whois: Query domain records, registrar, servers
- nslookup: Resolve domain names and query DNS record types
- dig: Advanced DNS queries
- DNSDumpster: Indirect DNS and subdomain record discovery
- Shodan.io: Search engine for open ports, banners, and vulnerabilities against public IP

## Passive Recon Use Cases
- Map external exposure without alerting target
- Find publicly available assets
- Understand scope of target before engaging

### WHOIS
Queries database for registration information in domain or IP
**Syntax:**
```
whois DOMAIN
whois IP_ADDRESS
```
**Finds following details:**
Registrar: Via which registrar was the domain name registered?
Contact info of registrant: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
Name Server: Which server to ask to resolve the domain name
Registrant contact: name and email
IP Block Owner: IP lookups and owner of address

### nslookup
Simple, fast DNS client for querying DNS record types
**Syntax:**
```
nslookup -type=RECORD_TYPE DOMAIN
nslookup -type=RECORD_TYPE DOMAIN DNS_SERVER
```

**Find IP Address of Domain:**
```
nslookup -type={SERVICE TYPE} {DOMAIN_NAME} {SERVER}
```
**Service Types:**
1. A = IPv4
2. AAAA = IPv6
3. CNAME = Canonical Name
4. MX = Mail Servers
5. SOA = Start of Authority
6. TXT = TXT Records

### Dig
More advanced DNS query tool, including TTL values, name, server, and time

**Syntax:**
```
dig DOMAIN RECORD_TYPE
dig @DNS_SERVER DOMAIN RECORD_TYPE
```
**Advanced DNS Queries and Functionality:**
```
dig @{SERVER} {DOMAIN_NAME} {SERVICE TYPE}
```
**Zone Transfer**
```
dig DOMAIN AXFR
```


### DNSDumpster
Online tool for passive subdomain discovery and DNS record query. 
**Returns:**
- Subdomains + IPs
- MX records
- TXT records
- DNS records

**How to use:**
1. Go to https://dnsdumpster.com on any browser
2. Enter target domain in search bar
3. Review output for search query

### Shodan.io 
Online search engine scanning internet to find open ports, service banners, SSL certificates, and OS versions.
**Returns**
- Open ports on public IP
- Service banners with name and version
- SSL certificates
- Associated CVEs and service versions
- Device type
  
```
hostname:TARGET-DOMAIN.com
ip:TARGET_PUBLIC_IP
org:"Target Organisation Name"
```
**How to use:**
1. Go to https://www.shodan.io
2. Search for target public IP, domain, or name
3. Review output and vulnerabilities

## PreRequisites:

**WHOIS Installation**
```
#Install WHOIS-Windows:
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco install whois

#Install WHOIS-Ubuntu/Linux (Debian-based):
sudo apt update && sudo apt install whois

#Install WHOIS-macOS (With Homebrew):
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install whois

#Verify WHOIS installed:
whois google.com | grep -i "domain name" && echo "SUCCESS: WHOIS is working" || echo "ERROR: WHOIS not working or not installed"
```

**Nslookup and Dig Installation**
```
# Install nslookup & dig - Windows:
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco install bind-toolsonly

# Install nslookup & dig - Ubuntu/Linux (Debian-based):
sudo apt update && sudo apt install dnsutils

# Install nslookup & dig - macOS (With Homebrew):
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install bind

# Verify nslookup installed:
nslookup google.com | grep -i "address" && echo "SUCCESS: nslookup is working" || echo "ERROR: nslookup not working or not installed"

# Verify dig installed:
dig google.com | grep -i "ANSWER SECTION" && echo "SUCCESS: dig is working" || echo "ERROR: dig not working or not installed"
```

## Practical Application:
Use Case:
- Review domain exposure with WHOIS before pen test
- Find vulnerable TXT records
- Check for open ports on public IP with Shodan
- Discover active subdomains

**Commands:**
```
# Check your public IP
curl -s ifconfig.me

# WHOIS your domain and your public IP
whois YOUR-DOMAIN.com
whois $(curl -s ifconfig.me)

# Pull all DNS records for your domain
dig YOUR-DOMAIN.com ANY
dig YOUR-DOMAIN.com A
dig YOUR-DOMAIN.com MX
dig YOUR-DOMAIN.com TXT

# Attempt zone transfer — should return REFUSED
dig YOUR-DOMAIN.com AXFR

# Use a specific resolver to confirm records are consistent
dig @8.8.8.8 YOUR-DOMAIN.com A
dig @1.1.1.1 YOUR-DOMAIN.com MX

# Check Shodan for your public IP (CLI)
# https://www.shodan.io/host/YOUR_PUBLIC_IP
```
