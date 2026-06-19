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
nmap -sV --version-intensity 5 TARGET_IP
nmap -sV --version-light TARGET_IP    # intensity 2
nmap -sV --version-all TARGET_IP      # intensity 9
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
```

**traceroute (--traceroute)**
Perform traceroute for network path mapping

**Use Cases:**
- Map network path to target
```
nmap --traceroute TARGET_IP
```

**Nmap Scripting Enginer (NSE)**
Lets Nmap run scripts against hosts

**Use Cases:**
-  Automate and run vulnerability checks
-  Brute-force attacks

**Syntax:**
```
# Run default scripts
nmap -sC TARGET_IP

# Run a specific script
nmap --script=SCRIPT_NAME TARGET_IP

# Run a category
nmap --script=CATEGORY TARGET_IP

# Example — run the http-date script
nmap --script "http-date" TARGET_IP
```

### Output Formats
- Normal (-oN): .nmap
- XML (-oX): .xml
- Greppable (-oG): .gnmap

**Syntax:**
```
nmap -oN OUTPUT_FILE.nmap TARGET_IP
nmap -oG OUTPUT_FILE.gnmap TARGET_IP
nmap -oX OUTPUT_FILE.xml TARGET_IP
nmap -oS OUTPUT_FILE.txt TARGET_IP
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
