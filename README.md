# THM-Network-Security-Protocols and Servers
Based off the TryHackMe Network Security Module

## Overview
- Protocols: HTTP, FTP, SMTP, POP3, and IMAP
- Attacks/Hardening: Sniffing, Man-in-the-middle, and Brute Force Attacks

## Contents
- [01-protocol-internals.md](01-protocol-internals.md): HTTP, FTP, SMTP, POP3, IMAP 
- [02-attacks-and-hardening.md](02-attacks-and-hardening.md): Sniffing, MITM, Hydra Brute-Force, SSH Hardening

## Prerequisites 
**Required Tools**
- telnet: banner grabbing (Root not required)
- hydra: brute force attacks (Root not required)
- tcpdump: packet capture (Root (sudo) required)
- ssh: SSH testing 

**Installation**
**Ubuntu**
```
sudo apt update
sudo apt install telnet tcpdump hydra openssh-client

which telnet tcpdump hydra ssh
```
**MacOS**
```
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install telnet hydra

which telnet tcpdump hydra ssh
```
**Windows**
```
# Install WSL (PowerShell as Administrator)
wsl --install

# Inside WSL (Ubuntu), run the Ubuntu instructions above
sudo apt update && sudo apt install telnet tcpdump hydra openssh-client
```

**Pre-check**
```
# 1. Identify your subnet and gateway
ip a
ip route | grep default

# 2. Scan for cleartext protocol ports across your network
nmap -p 21,23,25,80,110,143 --open 192.168.1.0/24
```

### Quick Reference
```
# Telnet — connect to any TCP service
telnet TARGET_IP PORT

# tcpdump — capture and read traffic
sudo tcpdump port 110 -A
sudo tcpdump -i eth0 -w capture.pcap

# Hydra — password attack
hydra -l USERNAME -P WORDLIST SERVICE://TARGET_IP

# SSH — secure remote access
ssh username@TARGET_IP

# TLS audit
nmap --script ssl-enum-ciphers -p 443 TARGET_IP
openssl s_client -connect TARGET_IP:443
```
