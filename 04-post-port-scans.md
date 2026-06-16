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


**Xmas Scan (-sX)**
Sets FIN, PSH, and URG flags

**Use Cases:**
- Stealh alternative to SYN-based scans
```
sudo nmap -sX TARGET_IP
sudo nmap -sX -p 1-1024 TARGET_IP
```

**Maimon Scan (-sM))**
Sends FIN/ACK Packet

**Use Cases:**
```
sudo nmap -sM TARGET_IP
```

**ACK Scan (-sA)**
Sends TCP ACK packet to each port

**Use Cases:**
-  Map firewall rules
-  Identify reachable ports
```
sudo nmap -sA TARGET_IP
sudo nmap -sA -p 1-1024 TARGET_IP
```

**Windows Scan (-sW)**
ACK scan inspecting TCP window field value

**Use Cases:**
- Getting open/closed detail from ACK responses
```
sudo nmap -sW TARGET_IP
```

**Custom TCP Scan (--scanflags)**
Manually sets combination of TCP flags

**Use Cases:**
- Testing firewall/IDS
- Bypass signature-based detection
```
# SYN + FIN (illegal combination — tests IDS behaviour)
sudo nmap --scanflags SYNFIN TARGET_IP

# SYN + RST (another unusual combination)
sudo nmap --scanflags SYNRST TARGET_IP

# URG + PSH + FIN (same as Xmas but via custom flag)
sudo nmap --scanflags URGPSHFIN TARGET_IP
```

**Spoof Source IPs (-S)**
Send packets with fake IP Source Address

**Use Cases:**
- Generate decoy traffic to confuse network logs
```
sudo nmap -S SPOOFED_IP -e INTERFACE -Pn TARGET_IP
```

**Decoy Scan (-D)**
Makes scan appear to come from multiple IP addresses

**Use Cases:**
- Test IDS to identify real IP source
- Red team testing/engagements
```
# Use 5 random decoy IPs
sudo nmap -D RND:5 TARGET_IP

# Specify exact decoy IPs (include ME for your real position)
sudo nmap -D DECOY1,DECOY2,ME TARGET_IP
```

**Idle/Zombie Scan (-sI)**
Blind Scan

**Use Cases:**
- Red team engagements/testing
```
sudo nmap -sI ZOMBIE_IP TARGET_IP
sudo nmap -sI ZOMBIE_IP:PORT TARGET_IP
```
**Requirements for Zombie Host:**
- Must be idle during scan
- Must have predictable IP ID counter with increments

**Fragmentation (-f)**
Splits TCP eader into small IP fragments

**Use Cases:**
- Testing if firewall correctly drops fragmented port scan traffic
```
# 8-byte fragments
sudo nmap -f TARGET_IP

# 16-byte fragments
sudo nmap -ff TARGET_IP

# Custom fragment size (must be a multiple of 8)
sudo nmap --mtu 16 TARGET_IP
```

**Source Port Spoofing (--source-port)**
Sets source port of scan packets to value

**Use Cases:**
- Bypass firewalls that trust traffic from specific ports
- Test own firewall rules
```
sudo nmap --source-port 53 TARGET_IP
sudo nmap --source-port 80 TARGET_IP
```

**Data Length Padding (--data-length)**
Appends random data to scan packets to change the size

**Use Cases:**
- Evade IDS signatures that match Nmap default packet sizes
```
sudo nmap --data-length 200 TARGET_IP
```

### Practical Application

**Commands:**
```
# Map your firewall rules with an ACK scan
sudo nmap -sA -p 1-1024 192.168.1.1

# Test Null, FIN, and Xmas against your router to see what your firewall reports
sudo nmap -sN 192.168.1.1
sudo nmap -sF 192.168.1.1
sudo nmap -sX 192.168.1.1

# Fragment packets and check if your IDS/firewall logs them
sudo nmap -f -sS 192.168.1.1

# Run a decoy scan in a lab environment
sudo nmap -D RND:5 192.168.1.50
```

**Important Things to Look For**
- Ports showing unfiltered in ACK scan
- Different Null/FIN/Xmas scan results than SYN scan
- IDS fails to alert fragmented scans

