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
-p PORT = Single Port
-p PORT1, PORT2 = Multiple Specific Ports

### Practical Application
**Use Cases:**
- Build host inventory of home network
- Discover forgotten/unkown devices connected to LAN
- Confirm all expected devices online

**Commands:**
```
# Step 1 — Identify your subnet
ip route | grep -v default | head -1

# Step 2 — ARP sweep to get the most complete host list
sudo nmap -PR -sn 192.168.1.0/24 -oG live-hosts.txt

# Step 3 — Extract just the IPs into a clean list
grep "Up" live-hosts.txt | awk '{print $2}' | tee hosts.txt

# Step 4 — Cross-check with ICMP and TCP probes to catch any ARP-silent hosts
sudo nmap -PE -PS22,80,443 -PA80 -sn 192.168.1.0/24

# Step 5 — Review MAC addresses to identify unknown devices
sudo nmap -PR -sn 192.168.1.0/24 | grep -E "MAC|report"
```
