### ping
```
# Send 5 packets and stop
ping -c 5 TARGET_IP

# Send 1 packet — fast online check
ping -c 1 TARGET_IP

# Ping sweep — find all live hosts on a subnet
for i in {1..254}; do
  ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i is UP"
done

# Ping with a specific packet size
ping -c 4 -s 1000 TARGET_IP
```
### traceroute
```
# Standard traceroute
traceroute TARGET_IP

# Skip DNS resolution (faster)
traceroute -n TARGET_IP

# Set maximum hops
traceroute -m 15 TARGET_IP

# TCP SYN-based probes — better at bypassing firewalls (requires root)
sudo traceroute -T -p 80 TARGET_IP
sudo traceroute -T -p 443 TARGET_IP

# ICMP-based probes (Linux)
sudo traceroute -I TARGET_IP

# Windows equivalent
tracert TARGET_IP
tracert -d TARGET_IP        # Skip DNS resolution
```
### telnet 
```
# HTTP — send a manual GET request
telnet TARGET_IP 80
GET / HTTP/1.1
Host: TARGET_IP
(press Enter twice)

# HTTP HEAD request — headers only, no body
telnet TARGET_IP 80
HEAD / HTTP/1.1
Host: TARGET_IP
(press Enter twice)

# SMTP — read banner and test relay
telnet TARGET_IP 25
EHLO test.com
MAIL FROM:<test@test.com>
RCPT TO:<user@target.com>
QUIT

# FTP — read banner
telnet TARGET_IP 21

# POP3 — read banner and authenticate
telnet TARGET_IP 110
USER username
PASS password
LIST
RETR 1
QUIT

# SSH — read banner without authenticating
telnet TARGET_IP 22

# Check if Telnet itself is open (should not be)
telnet TARGET_IP 23
```

### netcat
```
# Banner grab — verbose, numeric, no DNS
nc -nv TARGET_IP 22
nc -nv TARGET_IP 80
nc -nv TARGET_IP 21
nc -nv TARGET_IP 25
nc -nv TARGET_IP 110

# Send a manual HTTP GET request
nc -nv TARGET_IP 80
GET / HTTP/1.1
Host: TARGET_IP
(press Enter twice)

# Grab just the Server header
echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc -nv TARGET_IP 80 2>/dev/null | grep -i server

# Port scan a range without sending data
nc -nv -z TARGET_IP 20-30
nc -nv -z -w 1 TARGET_IP 1-1024

# Check a single port with timeout
nc -nv -z -w 1 TARGET_IP 23

# Listen on a local port (catch incoming connections)
nc -lvp 4444

# Transfer a file — receiver listens, sender connects
nc -lvp 9999 > received_file.txt       # Receiver
nc TARGET_IP 9999 < file_to_send.txt   # Sender

# UDP connection
nc -u TARGET_IP PORT
```

### Web Browser DevTools
```
# Open DevTools
F12
Ctrl + Shift + I    (Windows/Linux)
Cmd + Option + I    (macOS)

# View page source
Ctrl + U            (Windows/Linux)
Cmd + U             (macOS)

# Key tabs to check
Network tab  → HTTP requests, response headers, status codes, cookies
Console tab  → JavaScript errors, hardcoded values
Storage tab  → Cookies, localStorage, sessionStorage
Elements tab → Full HTML including hidden fields and comments
```

### Useful Single-Line Commands
```
# Check if a host is up with a one-packet ping
ping -c1 TARGET_IP &>/dev/null && echo "UP" || echo "DOWN"

# Fast LAN ping sweep with output
for i in {1..254}; do ping -c1 -W1 192.168.1.$i &>/dev/null && echo "192.168.1.$i"; done

# Grab banners from multiple ports on one host
for port in 21 22 23 25 80 110 143 443; do
  echo "=== Port $port ===" && echo "" | nc -nv -w 1 TARGET_IP $port 2>&1 | head -3
  echo ""
done

# Check if Telnet is open across a subnet
for ip in 192.168.1.{1..254}; do
  nc -nv -z -w 1 $ip 23 2>&1 | grep -i "open" && echo "TELNET OPEN: $ip"
done
```
