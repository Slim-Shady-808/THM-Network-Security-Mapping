## Overview
- HTTP: Port 80
- FTP: Port 21
- SMTP: Port 25
- POP3: Port 110
- IMAP: Port 143

#### HTTP
Text-based request/response

**Use Cases:**
- Banner grab web software version
- Inspect server cookies
- Identify header misconfigurations

**HTTP Commands**
- GET: Retrieve resource
- POST: Submit resource
- HEAD: Retrieve header
- PUT: Upload resource
- DELETE: Remove resource

**HTTP Quick Reference**
```
**Telnet**
telnet TARGET_IP 80

# GET request
GET / HTTP/1.1
Host: TARGET_IP
(press Enter twice)

# HEAD request 
HEAD / HTTP/1.1
Host: TARGET_IP
(press Enter twice)


**Netcat**
# Grab server header
echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc -nv TARGET_IP 80 2>/dev/null | grep -i server

# Check response headers
echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc TARGET_IP 80


**Curl**
# Response headers only
curl -sI http://TARGET_IP

# Full response 
curl -s http://TARGET_IP
```

**Important Information:**
- Server name and version
- Cookies

#### FTP
Transfer file content

**Use Cases:**
- Check accessible files via FTP
- Banner grabbing software and version
- Active Mode: Open connection on port 20
- Passive mode: Open connection to server-specified port

**FTP Commands:**
- USER username: input username
- PASS password: input pass
- ls/dir: list files
- get ...: download file
- put ...: upload file
- quit: close connection

**FTP Quick Reference**
```
**Telnet**
telnet TARGET_IP 21

**FTP**
ftp TARGET_IP

# When prompted:
Name: anonymous
Password: (leave blank or enter any email address)

ftp> ls
ftp> get filename
ftp> bye
```

**Important Information:**
- Banner reveals software and version
- Unwanted FTP on device

#### SMTP
Server to server email

**Use Cases:**
- Test openess of mail server
- Identify mail server software/version

**SMTP Commands:**
- EHLO domain: Greet server
- MAIL FROM:<address>: Specify sender 
- RCPT TO:<address>: Specify recipient
- QUIT: Close connection

 **SMTP Quick Reference**
 ```
telnet TARGET_IP 25

EHLO attacker.com
MAIL FROM:<sender@attacker.com>
RCPT TO:<recipient@target.com>
DATA
Subject: Test Email
From: sender@attacker.com
To: recipient@target.com

This is the body of the email.
.
QUIT
```

**Important Information:**
- Software/version revealed
- Port 25 open externally

#### POP3
Email/Mail Server

**Use Cases:**
- Cleartext credential transmission
- Confirm POP3S exists (not POP3)

**POP3 Commands:**
- USER username: Input username
- PASS password: Input password
- STAT: Show # of messages
- LIST: List all messages
- RETR N: Retrieve message N
- DELE N: Mark message N for deletion
- QUIT: Close connection

**POP3 Quick Reference**
```
telnet TARGET_IP 110

# Server responds with: +OK POP3 server ready
USER username
PASS password

# List messages
LIST

# Read message 1
RETR 1

# Close the connection
QUIT
```

**Important Information:**
- Visible credentials
- POP3 running (Should be POP3S)

#### IMAP
Manages email on server

**Use Cases:**
- List mailboxes and messages
- Make sure IMAPS in place (not IMAP)

**IMAP Commands:**
- TAG LOGIN username password: Login
- TAG LIST "" "": List mailboxes
- TAG SELECT INBOX: Open inbox
- TAG LOGOUT: End connection

**IMAP Quick Reference**
```
telnet TARGET_IP 143

a LOGIN username password
b LIST "" "*"
c SELECT INBOX
d FETCH 1 BODY[]
e LOGOUT
```

**Important Information:**
- Visible cleartext credentials
- IMAP running (Should be IMAPS)
- Accessible mailboxes w/o authentication


### Practical Application
**Use Cases:**
- Test accessible mail servers
- Verify FTP login is disabled

**Commands:**
```
# Scan your network for all cleartext protocol ports
nmap -p 21,25,80,110,143 --open 192.168.1.0/24

# HTTP — check security headers 
curl -sI http://YOUR-DOMAIN.com
curl -sI https://YOUR-DOMAIN.com | grep -iE "strict-transport|x-frame|content-security|x-content"

# FTP — check for anonymous login 
nmap --script ftp-anon -p 21 192.168.1.0/24

# FTP — manual anonymous login test
ftp 192.168.1.X
# Name: anonymous   Password: (leave blank)

# SMTP — test your mail server 
telnet YOUR_MAIL_SERVER 25
EHLO test.com
MAIL FROM:<outside@external.com>
RCPT TO:<anotherexternal@other.com>

# POP3 — verify cleartext POP3 is not running
nc -nv -z -w 1 YOUR_MAIL_SERVER 110

# IMAP — verify cleartext IMAP is not running
nc -nv -z -w 1 YOUR_MAIL_SERVER 143
```
