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
- Registrar
- Contact info of registrant
- Creation, update, and expiration dates
- Name Server
- IP Block Owner

### nslookup
Simple command-line DNS client for querying DNS record types
**Syntax:**
```
nslookup -type=RECORD_TYPE DOMAIN
nslookup -type=RECORD_TYPE DOMAIN DNS_SERVER
```

**Service Types:**
1. A = IPv4
2. AAAA = IPv6
3. CNAME = Canonical Name
4. MX = Mail Servers
5. SOA = Start of Authority
6. TXT = TXT Records
7. NS = Domain Name Servers

### Dig
More advanced DNS query tool, including TTL values, name server, and time

**Syntax:**
```
dig @DNS_SERVER DOMAIN RECORD_TYPE
```

**Zone Transfer**
```
dig ns DOMAIN
dig DOMAIN axfr @NS1.DOMAIN
```

### DNSDumpster
Online tool (https://dnsdumpster.com) for passive subdomain discovery and DNS record query. 
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
dig YOUR-DOMAIN.com TYPE

# Attempt zone transfer — should return REFUSED
dig YOUR-DOMAIN.com AXFR @NS1.DOMAIN

# Confirm records are consistent
dig @8.8.8.8 YOUR-DOMAIN.com A
dig @1.1.1.1 YOUR-DOMAIN.com MX

# Check Shodan for your public IP
# https://www.shodan.io/host/YOUR_PUBLIC_IP
```
