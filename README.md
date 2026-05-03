# THM-Network-Security-Passive-Reconnaissance
Based off the TryHackMe Network Security Module

PreRequisites:
```
#Install WHOIS-Windows:
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
choco install whois

#Install WHOIS-Ubuntu/Linux (Debian-based):
sudo apt update && sudo apt install whois

#Install WHOIS-macOS (With Homebrew):
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install whois

#Verify WHOIS installed:
whois google.com | grep -i "domain name" && echo "SUCCESS: WHOIS is working" || echo "ERROR: WHOIS not working or not installed"
```

Finding following details:
Registrar: Via which registrar was the domain name registered?
Contact info of registrant: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
Name Server: Which server to ask to resolve the domain name?

```whois {DOMAIN_NAME}
