### Overview
- ARP Scan: Discover hosts on local subnet
- ICMP Echo Scan: Ping scan
- ICMP Timestamp and Address Mask Scan
- TCP SYN Ping: Sends SYN packet to port
- TCP ACK Ping: Send ACK packet
- UDP Ping: Sends UDP probe

### Key Concepts
**-sn: Disable Port Scanning**
Perform Host Discovery Only

**Use Cases:**
- Getting fast list of live hosts
- Save live host list to a file

```
# Ping sweep with no port scan
nmap -sn SUBNET

# ARP sweep — save output for follow-on scans
sudo nmap -PR -sn 192.168.1.0/24 -oG live-hosts.txt
grep "Up" live-hosts.txt | awk '{print $2}' > hosts.txt
```

**ARP Scan (-PR)**
Sends ARP Request to every host (Can't be blocked by host firewall)

**Use Cases:**
- Discover devices blocking ICMP and TCP probes
- Build host list before port scanning

**Important Behaviours**
- Only works with local network
- Nmap automatically uses ARP
```
sudo nmap -PR -sn 192.168.1.0/24
```

**ICMP Echo Scan (-PE)**
Sends ICMP Echo Request Packets

**Use Cases:**
- Fast host sweep
- Confirm hosts are reachable
```
sudo nmap -PE -sn TARGET_IP
sudo nmap -PE -sn 192.168.1.0/24
```

**ICMP Timestamp Scan (-PP)**
Sends ICMP Timestamp Request Packets 

**Use Cases:**
- Bypass firewall rules blocking ICMP Echo
```
sudo nmap -PP -sn 192.168.1.0/24
```

**ICMP Address Mask Scan (-PM)**
Sends ICMP Address Mask Request Packets

**Use Cases:**
- Last resort ICMP probe if -PE and -PP blocked
```
sudo nmap -PM -sn 192.168.1.0/24
```

**TCP SYN Ping (-PS)**
Sends TCP SYN Packet to port

**Use Cases:**
- Discover hosts using TCP services
- Target specific open ports
```
# SYN ping on the default port (80)
sudo nmap -PS -sn TARGET_IP

# SYN ping on multiple ports
sudo nmap -PS22,80,443 -sn 192.168.1.0/24

# SYN ping on a port range
sudo nmap -PS1-1024 -sn TARGET_IP
```

**TCP ACK Ping (-PA)**
Sends TCP ACK Packet

**Use Cases:**
- Discover hosts when SYN packets blocked by firewall
```
sudo nmap -PA -sn TARGET_IP
sudo nmap -PA80,443 -sn 192.168.1.0/24
```

**UDP Ping (-PU)**
Sends UDP Packet to specified port

**Use Cases:**
- Discover hosts blocking al TCP and ICMP probes but open to UDP
```
sudo nmap -PU -sn TARGET_IP
sudo nmap -PU53,161 -sn 192.168.1.0/24
```

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
