# THM-Network-Security-Active-Reconnaissance
Based off the TryHackMe Network Security Module

## Overview
- Web Browser: Built in tools for HTTP responses, headers, and cookies
- Ping: Test reachability using ping requests
- Traceroute: Map network path
- Telnet: Connect to open TCP ports to read banners and services
- Netcat: TCP/UDP tool for banner grabbing, connections, port listening, and basic file transfer

## Key Concepts
### Web Browser
**Use Cases:**
- Find Missing Security Headers
- Inspect session cookies
- Check Javascript files for API keys and internal endpoints

**How to Open**
```
- F12 (Major Browsers)
- Ctrl + Shift + I (Windows/Linux)
- Cmd + Option + I (macOS)
```
**Key DevTools Tabs**
- Network: Shows HTTP requests and responses
- Console: Javascript log output and errors
- Storage: Cookies, localStorage, and sessionStorage
- Inspector/Elements: HTML

**Useful Browser Extensions**
- Wappalyzer
- FoxyProxy

### Ping
ICMP Echo Request packets 
**Use Cases**
- Confirm host reachability
- Perform subnet sweep
- Measure packet loss and latency
- Verify network connectivity
**Syntax**
```
ping -c COUNT TARGET_IP       (Linux/macOS)
ping -n COUNT TARGET_IP       (Windows)
```

### Traceroute
Maps network path between machine and a target

**Use Cases:**
- Map number of hops and routers between machine and target
- Identify where network traffic is filtered or dropped
- COnfirm outbound traffic exits through intended gateway
- Detect unexpected hops within LAN

**Syntax:**
```
traceroute TARGET_IP      (Linux/macOS)
tracert TARGET_IP         (Windows)
```

### Telnet 
Cleartext Remote Terminal Protocol (TCP Port 23)

**Use Cases**
- Grab service banners from HTTP, FTP, SMTP, POP3, and IMAP to identify software versions
- Send raw protocol commands
- Verify port 23 is not open
- Read SSH banner without authenticating
- Test SMTP relay configuration
- Confirm open ports

**Syntax**
```
telnet TARGET_IP PORT
```

### Netcat
Opens Raw TCP/UDP Connections
**Use Cases**
- Banner Grabbing TCP Services
- Check specific open port
- Transfer files between hosts
- Send raw UDP probes
- Scriptable
- Listening mode

**Syntax**
```
nc MACHINE_IP PORT

# Server Mode
nc -lvnp PORT
```
**Key Flags**
-n: Numeric only 
-v: Verbose
-v -v: Extra verbose
-l: Listen mode
-p PORT: Specific source port

## Practical Application
**Use Cases:**
- Confirm devices on network are online
- Map network path from machine to router
- Banner grab open services
- Verify telnet is not open

**Commands:**
```
# Confirm your gateway/router is online
ping -c 4 192.168.1.1

# Ping sweep your subnet to find all active hosts
for i in {1..254}; do
  ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i is UP"
done

# Map the path from your machine to the internet
traceroute 8.8.8.8

# Check for unexpected intermediate hops on your LAN
traceroute -n 192.168.1.1

# Banner grab your router's SSH service
nc -nv 192.168.1.1 22

# Banner grab your router's HTTP admin interface
nc -nv 192.168.1.1 80

# Check if Telnet is open on any device — should return nothing
nc -nv -z -w 1 192.168.1.1 23
```
**What to look for:**
- Telnet (Port 23) response on any device
- SSH Banners show outdated SSH Versions
- HTTP admin interface on unexpected ports
- Traceroute shows unexpected hops within LAN
- FTP banners on devices not expected to run FTP
 
## Prerequisites:
**Installation**
Debian/Ubuntu
```
sudo apt update
sudo apt install telnet netcat-openbsd

which ping traceroute telnet nc
```
MacOS
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install telnet netcat

which ping traceroute telnet nc
```

Windows
```
dism /online /Enable-Feature /FeatureName:TelnetClient

Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco install ncat
```
**Pre-Checks**
```
# 1. Confirm local IP and subnet
ip a

# 2. Identify your default gateway (router IP)
ip route | grep default

# 3. Confirm your machine can reach the gateway
```

**Permissions**
- ping: Works without root on most systems
- traceroute with SYN probes: Requires sudo on Linux
- traceroute with ICMP probes: Requires sudo on Linux
- Raw socket operations: Requires sudo
- Browser DevTools and netcat: No elevated privileges required
