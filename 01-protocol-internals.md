## Overview
- Telnet
- HTTP (Port 80)
- FTP: (Port 21)
- SMTP: (Port 25)
- POP3: (Port 110)
- IMAP: (Port 143)

#### Telnet
Protocol to connect to VM

**Use Cases:**
- Connect to remote system

**Syntax**
```
telnet TARGET_IP PORT
```

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
telnet TARGET_IP 80

# Once connected, send a GET request:
GET / HTTP/1.1
host: TARGET_IP
(press Enter twice)
```

#### FTP
Transfer file content

**Use Cases:**
- Check accessible files via FTP
- Banner grabbing software and version

**FTP Commands:**
- USER username: input username
- PASS password: input pass
- LIST: List files in directory
- RETR filename: Download file
- quit: close connection

**FTP Quick Reference**
```
**Telnet**
telnet TARGET_IP 21

# Read the banner — reveals FTP server software and version
# Send credentials
USER ....
PASS .....

**FTP**
ftp TARGET_IP

# When prompted:
Name: 
Password:
```

#### SMTP
Server to server email

**Use Cases:**
- Test openess of mail server
- Identify mail server software/version

**SMTP Commands:**
- HELO hostname: Greet server
- MAIL FROM:<address>: Specify sender 
- RCPT TO:<address>: Specify recipient
- DATA: Email body
- QUIT: Close connection

 **SMTP Quick Reference**
 ```
telnet TARGET_IP 25

# Once connected — the server sends a banner
HELO attacker.com
MAIL FROM:<attacker@attacker.com>
RCPT TO:<victim@target.com>
DATA
From: attacker@attacker.com
To: victim@target.com
Subject: Test

Hello, this is a test email.
.
QUIT
```

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
- QUIT: Close connection

**POP3 Quick Reference**
```
telnet TARGET_IP 110

[server] Mail Server POP3
USER frank
PASS D2xc9CgD
STAT
LIST
RETR 1
QUIT
```

#### IMAP
Manages email on server

**Use Cases:**
- List mailboxes and messages
- Make sure IMAPS in place (not IMAP)

**IMAP Commands:**
- TAG LOGIN username password: Login
- TAG LIST "" "": List mailboxes
- TAG EXAMINE INBOX: Check inbox for messages
- TAG FETCH N BODY[}: Retrieve content of message N
- TAG LOGOUT: End connection

**IMAP Quick Reference**
```
telnet TARGET_IP 143

c1 LOGIN frank D2xc9CgD
c2 LIST "" "*"
c3 EXAMINE INBOX
c4 FETCH 1 BODY[]
c5 LOGOUT
```

### Practical Application
**Use Cases:**
- Test accessible mail servers
- Verify FTP login is disabled

**Commands:**
```
# Connect to each protocol service
telnet TARGET_IP 23     # Telnet
telnet TARGET_IP 80     # HTTP
telnet TARGET_IP 21     # FTP
telnet TARGET_IP 25     # SMTP
telnet TARGET_IP 110    # POP3
telnet TARGET_IP 143    # IMAP

# FTP client — test anonymous login
ftp TARGET_IP
# Name: anonymous
# Password: (blank)
```
