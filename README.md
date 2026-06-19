# THM-Network-Security-Nmap
Based off the TryHackMe Network Security Module

## Overview
- Live Host Discovery: Determine which hosts are online with ARP, ICMP, TCP, and UDP. See [01-live-host-discovery](01-live-host-discovery.md)
- Basic Port Scans: Identify open, closed, and filtered ports with TCP Connect, TCP SYN, and UDP scans. See [02-basic-port-scans](02-basic-port-scans.md)
- Advanced Port Scans: Bypass firewalls with Null, FIN, Xmas, ACK, Window, and Idle/Zombie Scans. See [03-advanced-port-scans](03-advanced-port-scans.md)
- Post Port Scans: Find service versions, OS, NSE Scripts, and save outputs in multiple formats. See [04-post-port-scans](04-post-port-scans.md)



## Prerequisites 
**Installation**
**Ubuntu/Debian**
```
sudo apt update
sudo apt install nmap

nmap --version
```
**MacOS**
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install nmap

nmap --version
```
**Windows**
```
# Download from: https://nmap.org/download.html
# Run the .exe installer
```
**Pre-Checks**
```
# 1. Find your local subnet
ip route | grep -v default | head -1

# 2. Confirm nmap can reach your gateway
sudo nmap -PR -sn $(ip route | grep default | awk '{print $3}')

# 3. Create an output directory for results
mkdir -p ~/nmap-results
```

**Permissions**
- TCP Connect scan (-sT): No root required — uses the OS network stack
- TCP SYN scan (-sS): Requires sudo — sends raw packets
- UDP scan (-sU): Requires sudo — sends raw packets
- OS detection (-O): Requires sudo
- ARP scan (-PR): Requires sudo — only works on local subnet
- All ICMP probes (-PE, -PP, -PM): Require sudo
- NSE scripts: Most work without root; some (broadcast-*, raw socket scripts) require sudo
- Advanced scans (Null, FIN, Xmas, Idle, spoofing): All require sudo


### Contents
- [01-live-host-discovery.md](01-live-host-discovery.md)
- [02-basic-port-scans.md](02-basic-port-scans.md)
- [03-advanced-port-scans.md](03-advanced-port-scans.md)
- [04-post-port-scans.md](04-post-port-scans.md)
