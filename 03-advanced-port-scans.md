## Overview
- Null: Send packets with no TCP flags
- FIN: Send packets with only FIN flag
- Xmas: Sends packet with FIN, PSH, and URG flags
- Maimon Scan: FIN/ACK scan
- ACk Scan: Check reachable ports
- Window Scan: Use TCP to see if port open or closed
- Custom TCP Scan: Set TCP flags
- Spoofing and Decoys: Hide scanning IP
- Idle/Zombie Scan: Scan hiding real IP 

### Key Concepts
**Null Scan (-sN)**
Sends TCP packet with no flags set

**Use Cases**
- Scan targets blocking SYN packets
```
sudo nmap -sN TARGET_IP
```

**FIN Scan (-sF)**
Send TCP packet with only FIN flag

**Use Cases:**
- Stealth scan use cases
```
sudo nmap -sF TARGET_IP
```

**Xmas Scan (-sX)**
Sets FIN, PSH, and URG flags

**Use Cases:**
- Stealh alternative to SYN-based scans
```
sudo nmap -sX TARGET_IP
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
sudo nmap --scanflags RSTSYNFIN TARGET_IP
sudo nmap --scanflags URGACKPSHRSTSYNFIN TARGET_IP
```

**Spoof Source IPs (-S)**
Send packets with fake IP Source Address

**Use Cases:**
- Generate decoy traffic to confuse network logs
```
sudo nmap -S SPOOFED_IP TARGET_IP

sudo nmap -e NET_INTERFACE -Pn -S SPOOFED_IP TARGET_IP

#Spoof MAC Address
sudo nmap --spoof-mac SPOOFED_MAC TARGET_IP
```

**Decoy Scan (-D)**
Makes scan appear to come from multiple IP addresses

**Use Cases:**
- Test IDS to identify real IP source
- Red team testing/engagements
```
sudo nmap -D DECOY1,DECOY2,ME,DECOY3 TARGET_IP

# Use random decoy IPs
sudo nmap -D RND,RND,ME,RND TARGET_IP
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
sudo nmap -f TARGET_IP      # 8-byte fragments
sudo nmap -ff TARGET_IP     # 16-byte fragments
```

**Source Port Spoofing (--source-port)**
Sets source port of scan packets to value

**Use Cases:**
- Bypass firewalls that trust traffic from specific ports
- Test own firewall rules
```
sudo nmap --source-port 53 TARGET_IP
```

**Data Length Padding (--data-length)**
Appends random data to scan packets to change the size

**Use Cases:**
- Evade IDS signatures that match Nmap default packet sizes
```
sudo nmap --data-length 120 TARGET_IP
```

### Practical Application

**Important Things to Look For**
- Ports showing unfiltered in ACK scan
- Different Null/FIN/Xmas scan results than SYN scan
- IDS fails to alert fragmented scans
