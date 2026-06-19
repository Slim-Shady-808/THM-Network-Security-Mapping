### Web Browser
```
# Open Developer Tools
Ctrl + Shift + I        (PC)
Option + Command + I    (Mac)

# Find JavaScript files
Developer Tools → Sources → script.js
```

### ping
```
# Linux/macOS — send specific number of packets
ping -c COUNT TARGET_IP

# Windows
ping -n COUNT TARGET_IP

# Example from the room
ping -c 10 MACHINE_IP
```
### traceroute
```
# Linux/macOS
traceroute TARGET_IP
traceroute tryhackme.com

# Windows
tracert TARGET_IP
```
### telnet 
```
# Connect to any TCP port
telnet MACHINE_IP PORT

# HTTP banner grab — example from the room
telnet MACHINE_IP 80
GET / HTTP/1.1
host: telnet
(press Enter twice)
```

### netcat
```
# Client mode — connect to a service
nc MACHINE_IP PORT

# Example from the room
nc MACHINE_IP 21

# Server/listener mode
nc -lvnp PORT

# Note: p must come immediately before the port number
# Correct:   nc -lvnp 4444
# Incorrect: nc -lvpn 4444
```

