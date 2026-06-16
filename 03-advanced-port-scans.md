## Overview
- Null, FIN, Xmas Scans: Send packets with no TCP flags
- Maimon Scan: FIN/ACK scan
- ACk Scan: Check reachable ports
- Window Scan: Use TCP to see if port open or closed
- Custom TCP Scan: Set TCP flags
- Spoofing and Decoys: Hide scanning IP

### Key Concepts
**Null Scan (-sN)**
Sends TCP packet with no flags set

**Use Cases**
- Scan targets blocking SYN packets
```
sudo nmap -sN TARGET_IP
sudo nmap -sN -p 1-1000 TARGET_IP
```

**FIN Scan (-sF)**
Send TCP packet with only FIN flag

**Use Cases:**
- Stealth scan use cases
```
sudo nmap -sF TARGET_IP
sudo nmap -sF -p 22,80,443 TARGET_IP
```

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

### Practical Application
**Use Cases:**
- Audit router for unexpected open ports
- Scan NAS or media server
- Check all devices for open Telnet or insecure FTP
- Confirm SNMP
  
**Commands:**
```
# Quick SYN scan of your router — most important ports
sudo nmap -sS -p 22,23,53,80,443,8080,8443 192.168.1.1

# Full port scan of a specific device
sudo nmap -sS -p- -T4 192.168.1.50 -oA results/full-scan-device

# UDP audit of your router for SNMP and DNS
sudo nmap -sU -p 53,67,68,161,162,500 192.168.1.1

# Scan entire subnet for Telnet — should return nothing
nmap -p 23 --open 192.168.1.0/24

# Scan entire subnet for FTP
nmap -p 21 --open 192.168.1.0/24

# Fast audit of all hosts — top 100 ports
sudo nmap -sS -F 192.168.1.0/24 -oA results/fast-subnet-scan
```

**Important Things to Look For**
- Port 23 open on any device (disable)
- Port 21 open (disable)
- Port 161 open
- Port 8080 or 8443 open
- Filtered ports
