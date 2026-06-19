## Overview
- Port States: Open, closed, filtered, unfiltered, open|filtered, closed|filtered
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
```

**TCP SYN Scan (-sS)**
Sends SYN packet w/o completing handshake

**Use Cases:**
- Default Nmap scan
- Audit network
```
sudo nmap -sS TARGET_IP
```

**UDP Scan (-sU)**
Sends UDP packets to target ports

**Use Cases:**
- Discover DNS, SNMP, DHCP, TFTP, and NTP Services
- Audit Network Devices
```
sudo nmap -sU TARGET_IP
```

#### Port Specification Flags
- -p PORT = Single Port
- -p PORT1, PORT2 = Multiple Specific Ports
- -p START-END = Port Range
- -p- = All 65535 Ports
- -F = Fast mode
- --top-ports N = Top N most common ports
- -p U:Port,T:Port = Specify protocol per port

#### Port Flags
- -p PORT = Scan single port
- -p PORT1, PORT2 = Scan specific ports
- -p PORT1-PORT2 = Scan port range
- -p- = scan all ports
- -F = Fast mode
- --top-ports N = Scan the N most common ports

#### Timing 
- -T0 = Paranoid
- -T1 = Sneaky
- -T2 = Polite
- -T3 = Normal
- -T4 = Aggressive
- -T5 = Insane

### Verbosity
```
nmap -v TARGET_IP     # Verbose output
nmap -vv TARGET_IP    # More verbose
nmap -d TARGET_IP     # Debugging level 1
nmap -dd TARGET_IP    # Debugging level 2
```

### Practical Application
**Use Cases:**
- Audit router for unexpected open ports
- Scan NAS or media server
- Check all devices for open Telnet or insecure FTP
- Confirm SNMP


**Important Things to Look For**
- Port 23 open on any device (disable)
- Port 21 open (disable)
- Port 161 open
- Port 8080 or 8443 open
- Filtered ports
