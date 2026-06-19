### whois
```
# Domain registration lookup
whois DOMAIN.com

# IP address ownership lookup
whois TARGET_IP
```

### nslookup
```
# Basic syntax
nslookup OPTIONS DOMAIN_NAME SERVER

# A record — IPv4 address
nslookup -type=A tryhackme.com

# AAAA record — IPv6 address
nslookup -type=AAAA tryhackme.com

# MX records — mail servers
nslookup -type=MX tryhackme.com

# TXT records
nslookup -type=TXT tryhackme.com

# Query using a specific DNS server
nslookup -type=A tryhackme.com 1.1.1.1
nslookup -type=A tryhackme.com 8.8.8.8

# Flag question — TXT record lookup
nslookup -type=txt thmlabs.com```

```
### Dig
```
# MX record lookup
dig tryhackme.com MX

# Query using a specific DNS server
dig @1.1.1.1 tryhackme.com MX
```

### Online Tools 
```
#DNS Record Discovery
htts://dnsdumpster.com

#Shodan
Shodan.io
```

### Useful Single-Line Commands
```
# Check your own public IP
curl -s ifconfig.me

# Pull all common DNS record types for a domain in one go
for type in A AAAA MX TXT NS CNAME; do echo "=== $type ==="; dig DOMAIN.com $type +short; done

# Find subdomains via certificate transparency
curl -s "https://crt.sh/?q=%.DOMAIN.com&output=json" | python3 -m json.tool | grep "name_value" | sort -u
```
