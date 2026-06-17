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
- netcat: TCP/ banner grabbing (Root not required)
- ftp: file transfer (Root not required)
- curl: HTTP header retrieval (Root not required)
- openssl: TLS testing (Root (sudo) required)
- hydra: brute force attacks (Root not required)
- tcpdump: packet capture (Root (sudo) required)
- ssh: SSH testing 
- nmap: Scripts (Root not required)

**Installation**
**Ubuntu**
```
# Core protocol interaction tools
sudo apt update
sudo apt install telnet netcat-openbsd ftp curl openssl

# Attack and audit tools
sudo apt install hydra tcpdump openssh-client nmap

# Verify all tools are available
which telnet nc ftp curl openssl hydra tcpdump ssh nmap
```
**macOS**
```
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Protocol interaction tools
brew install telnet netcat

# Attack and audit tools
brew install hydra nmap

# Verify
which telnet nc curl openssl hydra tcpdump ssh nmap
```
**Windows**
```
# Install WSL (PowerShell as Administrator)
wsl --install

# Inside WSL (Ubuntu), run the Ubuntu instructions above
sudo apt update
sudo apt install telnet netcat-openbsd ftp curl openssl hydra tcpdump openssh-client nmap

**or**

# Install Chocolatey if not already installed
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install available tools
choco install nmap curl openssl ncat

# Enable Telnet client
dism /online /Enable-Feature /FeatureName:TelnetClient

# Enable built-in SSH client (PowerShell as Administrator)
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
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
# Manual protocol interaction
telnet TARGET_IP 80         # HTTP
telnet TARGET_IP 21         # FTP
telnet TARGET_IP 25         # SMTP
telnet TARGET_IP 110        # POP3
telnet TARGET_IP 143        # IMAP

# Capture cleartext credentials on the wire
sudo tcpdump -i eth0 -w capture.pcap
tcpdump -r capture.pcap -A | grep -iE "user|pass|login|auth"

# Hydra brute-force
hydra -l USER -P wordlist.txt ftp://TARGET_IP
hydra -l USER -P wordlist.txt ssh://TARGET_IP
hydra -l USER -P wordlist.txt pop3://TARGET_IP

# TLS audit
nmap --script ssl-enum-ciphers -p 443 TARGET_IP
openssl s_client -connect TARGET_IP:443
```
