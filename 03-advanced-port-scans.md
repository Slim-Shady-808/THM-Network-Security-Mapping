## Overview
- Port Scans: Open, closed, filtered, unfiltered, open|filtered, closed|filtered
- TCP Connect Scan: Three-way handshake
- TCP Syn Scan: Requires root
- UDP Scan: Proves UDP Ports (Useful for DNS, SNMP, and DHCP Services)

### Key Concepts
**TCP Fundamentals**
- SYN = Initiate Connection
- ACK = Confirm data receipt
- RST = Terminate connection (Abrupt)
- FIN = Terminate connection (Not Abrupt)
- PSH = Reciever passes data to app immediately
- URG = Data marked as urgent

**Port States**
- Open = Service actively listening and accepting connections on this port
- Closed = Port accessible but no service listening
- Filtered = Nmap being blocked by firewall
- Unfiltered = Port accessible but NMAP cannot determine open or closed
- Open|filtered = Nmap cannot tell if port is open or filtered
- Closed|filtered = Nmap cannot tell if port is closed or filtered

**TCP Connect Scan (-sT)**
Complete three-way handshake

**Use Cases:**
- Scan without root privileges
- Confirm open ports (Unstealthily)
```
nmap -sT TARGET_IP
nmap -sT -p PORT TARGET_IP
```

**TCP SYN Scan (-sS)**
Sends SYN packet w/o completing handshake

**Use Cases:**
- Default Nmap scan
- Audit network
```
sudo nmap -sS TARGET_IP
sudo nmap -sS -p PORT TARGET_IP
```

**UDP Scan (-sU)**
Sends UDP packets to target ports

**Use Cases:**
- Discover DNS, SNMP, DHCP, TFTP, and NTP Services
- Audit Network Devices
```
sudo nmap -sU TARGET_IP
sudo nmap -sU -p PORT TARGET_IP
```

#### Port Specification Flags
- -p PORT = Single Port
- -p PORT1, PORT2 = Multiple Specific Ports
- -p START-END = Port Range
- -p- = All 65535 Ports
- -F = Fast mode
- --top-ports N = Top N most common ports
- -p U:Port,T:Port = Specify protocol per port

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
