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
ping TARGET_IP
ping -c COUNT TARGET_IP
```

**Key Flags**
-c COUNT (Linux/macos): Send specific number of packets
-n COUNT (Windows): Send specific number of packets
-i INTERVAL (Linux/macos): Wait interval between each packet
-s SIZE (Linux/macos): Set packet payload size
-t TTL (Linux/macos): Set Time-to-Live value 

**Interpret Results**
- Reply recieved: ICMP not blocked, host is up
- Request timeout: Host down or firewall dropping ICMP
- Destination host unreachable: no route to host exists

### Traceroute
Maps network path between machine and a target

**Use Cases:**
- Map number of hops and routers between machine and target
- Identify where network traffic is filtered or dropped
- COnfirm outbound traffic exits through intended gateway
- Detect unexpected hops within LAN

**Syntax:**
```
traceroute TARGET_IP          (Linux/macOS)
tracert TARGET_IP             (Windows)
```
**Key Flags:**
-n: Do not resolve hostnames 
-m MAX_HOPS: Set max number of hops
-q QUERIES: Number of probe packets per hop
-I: Use ICMP instead of UDP (Linux)
-T: Use TCP SYN instead of UDP (Linux)

**Interpret Results**
- IP Address each line: Router at that hop and its round trip time
- * * *: Hop unresponsive means either firewall drops probes, or the router is unconfigured for replies
- Repeated IP: Routing loop

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

**Important Banner Information**
- HTTP: Server header reveal web software and version
- SMTP: Banner line reveals software
- FTP: Banner includes server name and version
- SSH: Banner includes OpsenSSH Versions
- POP3/IMAP: Banner reveals mail server software

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
nc [options] TARGET_IP PORT
```
**Key Flags**
-n: Numeric only 
-v: Verbose
-v -v: Extra verbose
-u: Use UDP 
-l: Listen mode
-p PORT: Specific source port
-w TIMEOUT: Timeout for idle connections
-z: Zero I/O mode

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
# telnet and netcat
sudo apt update
sudo apt install telnet netcat-openbsd curl

# Verify all tools are available
which ping traceroute telnet nc curl
```

Windows
```
# Enable Telnet client via PowerShell (Run as administrator)
dism /online /Enable-Feature /FeatureName:TelnetClient
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
