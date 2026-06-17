## Overview
- Service Detection (-sV): Probe open ports to find software name/version
- OS Detection (-O): Map target OS and version
- Traceroute (--traceroute): Map network path to target
- Nmap Scripting Engine (NSE): Run scripts for automation, brute-force, and vulnerability checks
- Output Formats: Save scan results with Normal, XML, and Greppable

### Key Concepts
**Service and Version Detection (-sV)**
Probe each open port

**Use Cases**
- Identify exact software version on each open port
- Confirm if service is running on port or firewall redirect
- Detect outdated software across network
```
nmap -sV TARGET_IP
nmap -sV --version-intensity LEVEL TARGET_IP
```
**version Intesity Levels**
0 = --version-light 
5 = Balanced
9 = --version-all

**OS Detection (-O)**
Analyze differences in TCP/IP stack 

**Use Cases:**
- Confirm OS of devices
- Identify end-of-life OSs
- Scope pen test
```
sudo nmap -O TARGET_IP
sudo nmap -O --osscan-guess TARGET_IP
```
**Useful OS Detection Flags**
- -O = Enable OS detection
- --osscan-guess = Guess aggressively when Nmap isn't confident
- --osscan-limit = Only attempt OS detection on hosts with at least one open + closed port

**traceroute (--traceroute)**
Perform traceroute for network path mapping

**Use Cases:**
- Map network path to target
```
nmap --traceroute TARGET_IP
nmap -A TARGET_IP         # -A includes traceroute automatically
```

**Aggressive Scan (-A)**
Convience flag for OS detection

**Use Cases:**
- Quick full-detail scan
```
sudo nmap -A TARGET_IP
sudo nmap -A -p- TARGET_IP    # Aggressive scan on all ports
```

**Nmap Scripting Enginer (NSE)**
Lets Nmap run scripts against hosts

**Use Cases:**
-  Automate and run vulnerability checks
-  Brute-force attacks

**Syntax:**
```
# Run default scripts (equivalent to --script=default)
nmap -sC TARGET_IP

# Run a specific script
nmap --script SCRIPT_NAME TARGET_IP

# Run multiple scripts
nmap --script SCRIPT1,SCRIPT2 TARGET_IP

# Run all scripts in a category
nmap --script CATEGORY TARGET_IP

# Run all scripts matching a pattern
nmap --script "http-*" TARGET_IP
```

**Common Scripts:**
```
# HTTP — enumerate web server details
nmap --script http-headers -p 80,443 TARGET_IP
nmap --script http-auth-finder -p 80,443 TARGET_IP
nmap --script http-default-accounts -p 80,8080,8443 TARGET_IP
nmap --script "http-vuln*" -p 80,443 TARGET_IP

# SSH — audit configuration
nmap --script ssh-auth-methods -p 22 TARGET_IP
nmap --script ssh2-enum-algos -p 22 TARGET_IP

# FTP — check anonymous login
nmap --script ftp-anon -p 21 TARGET_IP

# SMB — enumerate shares and check for vulnerabilities
nmap --script smb-enum-shares -p 445 TARGET_IP
nmap --script smb-vuln-ms17-010 -p 445 TARGET_IP

# SSL/TLS — audit cipher strength
nmap --script ssl-enum-ciphers -p 443 TARGET_IP

# Vulnerability scan across all services
nmap --script vuln TARGET_IP
```

### Output Formats
- Normal (-oN): .nmap
- XML (-oX): .xml
- Greppable (-oG): .gnmap

**Syntax:**
```
# Save in normal format
nmap -oN output.nmap TARGET_IP

# Save in XML format
nmap -oX output.xml TARGET_IP

# Save in greppable format
nmap -oG output.gnmap TARGET_IP

# Save all three simultaneously (recommended)
nmap -oA scan-results TARGET_IP
```

### Practical Application
**Use Cases:**
- Identify sotware versions
- Run vulnerability scripts

**Commands:**
```
# Full audit of your router 
sudo nmap -A --script vuln 192.168.1.1 -oA results/router-$(date +%F)

# Service version audit
sudo nmap -sV -iL hosts.txt -oA results/services-$(date +%F)

# SSH audit on all devices
nmap --script ssh-auth-methods,ssh2-enum-algos -p 22 -iL hosts.txt

# Anonymous FTP login check across subnet
nmap --script ftp-anon -p 21 192.168.1.0/24

# HTTP security header check 
nmap --script http-headers -p 80,443,8080,8443 192.168.1.0/24

# TLS audit on HTTPS 
nmap --script ssl-enum-ciphers -p 443 192.168.1.0/24

# Check for default credentials on web admin panels
nmap --script http-default-accounts -p 80,8080,8443 192.168.1.0/24
```

**Important Things to Look For**
- Outdated service versins
- SSH allowing weak passwords
- Old TLS versions
