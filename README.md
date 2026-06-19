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
nslookup OPTIONS DOMAIN_NAME SERVER
```

**Service Types:**
1. A = IPv4
2. AAAA = IPv6
3. CNAME = Canonical Name
4. MX = Mail Servers
5. SOA = Start of Authority
6. TXT = TXT Records

### Dig
More advanced DNS query tool, including TTL values, name server, and time

**Syntax:**
```
dig DOMAIN_NAME TYPE
dig @SERVER DOMAIN_NAME TYPE
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

**How to use:**
1. Go to https://www.shodan.io
2. Search for target public IP, domain, or name
3. Review output and vulnerabilities

## PreRequisites:
**Installation**
**Ubuntu/Debian**
```
sudo apt update
sudo apt install whois dnsutils

which whois nslookup dig
```
**MacOS**
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install whois

which whois nslookup dig
```
**Windows**
```
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco install bind-toolsonly
```
**who is installation**
```
# Download from: https://learn.microsoft.com/en-us/sysinternals/downloads/whois
whois.exe DOMAIN.com
```
**Pre-Checks**
```
# 1. Note your own public IP
curl -s ipconfig.me

# 2. Confirm dig can query your domain
dig YOUR-DOMAIN.com ANY

# 3. Check your domain's WHOIS privacy status
whois YOUR-DOMAIN.com | grep -i "registrant"
```

## Practical Application:
Use Case:
- Review domain exposure with WHOIS before pen test
- Find vulnerable TXT records
- Check for open ports on public IP with Shodan
- Discover active subdomains

