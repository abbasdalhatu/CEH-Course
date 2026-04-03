## WEEK 1: Introduction to Ethical Hacking & Lab Setup

### 📖 Lecture Notes

#### Topics Covered
- What is Ethical Hacking?
- Types of Hackers: White Hat, Gray Hat, Black Hat
- The Five Phases of Ethical Hacking
- Information Security Controls: Types and Methods
- Information Security Attacks: Types and Vectors
- Creating a Security Baseline
- Lab Environment Setup

#### Key Concepts

**Ethical Hacker Definition:**
An ethical hacker (or white-hat hacker) is a security professional who attempts to break into systems using the same techniques as malicious hackers, but with permission from the system owner and for defensive purposes.

**Types of Security Tests:**
1. **Vulnerability Assessment:** Identifying and ranking vulnerabilities without exploitation
2. **Penetration Testing:** Actively exploiting vulnerabilities to assess security
3. **Red Team Exercise:** Full-simulation attack mimicking real threat actors
4. **Security Audit:** Compliance-based security review

**The Five Phases of Hacking:**
1. **Reconnaissance** - Information gathering (passive/active)
2. **Scanning** - Port discovery, service identification
3. **Gaining Access** - Exploitation of vulnerabilities
4. **Maintaining Access** - Persistence mechanisms (backdoors, rootkits)
5. **Covering Tracks** - Log manipulation, evidence destruction

**Legal Framework:**
- Computer Fraud and Abuse Act (CFAA)
- Digital Millennium Copyright Act (DMCA)
- General Data Protection Regulation (GDPR)
- Always obtain written permission before testing

#### Essential Terminology
- **Exploit:** Code that takes advantage of a vulnerability
- **Payload:** Malicious code delivered by an exploit
- **Vulnerability:** Weakness in a system
- **Threat:** Potential danger to a system
- **Risk:** Likelihood of a threat exploiting a vulnerability

---

### 🔧 Practical Lab 1: Setting Up Your Hacking Lab

#### Objectives
- Install and configure VirtualBox/VMware
- Install Kali Linux in a virtual machine
- Install Metasploitable 2 (vulnerable target)
- Verify network connectivity between VMs
- Take baseline snapshots

#### Step-by-Step Instructions

**Step 1: Install VirtualBox**
```
1. Download VirtualBox: https://www.virtualbox.org/wiki/Downloads
2. Install VirtualBox and Extension Pack
3. Create a new virtual machine
```

**Step 2: Install Kali Linux**
```
1. Download Kali Linux VM Image: https://www.kali.org/get-kali/#kali-virtual-machines
2. Import the VM into VirtualBox
3. Configure network adapter to NAT Network or Host-Only
4. Start Kali Linux
5. Default credentials: kali / kali
6. Update the system:
   sudo apt update && sudo apt upgrade -y
```

**Step 3: Install Metasploitable 2**
```
1. Download Metasploitable 2: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
2. Extract the VM files
3. Import into VirtualBox
4. Configure network to match Kali Linux network
5. Start Metasploitable 2
6. Default credentials: msfadmin / msfadmin
```

**Step 4: Verify Connectivity**
```bash
# In Kali Linux, verify you can reach Metasploitable:
ifconfig                    # Find your IP address
ping <Metasploitable_IP>    # Should receive replies

# Test basic service on Metasploitable:
nmap -sV <Metasploitable_IP>
```

**Step 5: Take a Snapshot**
```
1. In VirtualBox, select each VM
2. Click "Snapshots" > "Take Snapshot"
3. Name: "Baseline - Clean Install"
4. This allows you to restore to a clean state anytime
```

#### Expected Outcomes
- Kali Linux VM running and updated
- Metasploitable 2 VM accessible from Kali
- Network connectivity confirmed
- Snapshots created for both VMs

---

### 📝 Assignment 1

**Part A: Short Answer (20 points each)**
1. Explain the difference between a vulnerability assessment and a penetration test.
2. Describe the five phases of ethical hacking with an example of each.
3. Why is written permission critical before conducting any security assessment?
4. What is the difference between a threat, a vulnerability, and a risk?
5. List three legal frameworks that affect ethical hacking and briefly explain each.

**Part B: Lab Documentation (40 points)**
- Submit screenshots of your lab setup
- Document your network configuration
- Show connectivity between Kali and Metasploitable
- Include your nmap scan results

**Submission Format:** PDF document, 3-5 pages minimum.

---

---

## WEEK 2: Footprinting and Reconnaissance

### 📖 Lecture Notes

#### Topics Covered
- Footprinting Methodology
- Passive vs. Active Reconnaissance
- Open Source Intelligence (OSINT)
- Search Engine Hacking (Google Dorking)
- Social Media Reconnaissance
- Website Reconnaissance
- Email Tracking and Reconnaissance
- DNS Enumeration

#### Key Concepts

**Footprinting Defined:**
Footprinting is the process of gathering all possible information about a target organization's systems, networks, and infrastructure. This is the FIRST and MOST CRITICAL phase of hacking.

**Information Gathered During Footprinting:**
- Domain names and IP address ranges
- Employee names, email formats, organizational structure
- Technologies in use (web servers, frameworks)
- Network topology
- Physical locations
- Job postings (reveal technologies used)

**Passive Reconnaissance:**
Gathering information without directly interacting with the target's systems.

Tools and Techniques:
- WHOIS lookups
- DNS enumeration
- Search engine queries
- Social media analysis
- Archive.org (Wayback Machine)
- Job boards and press releases

**Active Reconnaissance:**
Directly interacting with the target's systems (more likely to be detected).

Tools and Techniques:
- Port scanning
- Service enumeration
- SNMP queries
- Network mapping

#### Essential Tools

| Tool | Purpose |
|------|---------|
| whois | Domain registration information |
| nslookup / dig | DNS queries |
| theHarvester | Email and subdomain discovery |
| Maltego | Visual link analysis |
| Shodan | IoT/device search engine |
| Google Dorks | Advanced search queries |
| recon-ng | Modular reconnaissance framework |

#### Google Dorking Examples
```
site:target.com filetype:pdf          # Find PDF files
site:target.com inurl:admin           # Find admin pages
site:target.com intitle:"index of"    # Find directory listings
site:target.com ext:sql               # Find SQL files (possible data exposure)
site:pastebin.com "target.com"        # Find mentions on Pastebin
```

---

### 🔧 Practical Lab 2: Reconnaissance on Your Lab Target

#### Objectives
- Perform passive reconnaissance on Metasploitable
- Use OSINT tools to gather information
- Perform DNS enumeration
- Document all findings

#### Step-by-Step Instructions

**Step 1: WHOIS Lookup**
```bash
# Query WHOIS information (use a public domain for legal practice)
whois example.com
```

**Step 2: DNS Enumeration**
```bash
# Basic DNS queries
nslookup example.com
dig example.com ANY
dig @8.8.8.8 example.com

# Subdomain enumeration using gobuster
gobuster dns -d example.com -w /usr/share/wordlists/dirb/common.txt
```

**Step 3: theHarvester**
```bash
# Gather emails and subdomains
theHarvester -d example.com -l 500 -b google
theHarvester -d example.com -b bing
```

**Step 4: Shodan Reconnaissance**
```
1. Create free account: https://www.shodan.io/
2. Search for: "apache" or specific technologies
3. Search for your own public IP to see what Shodan knows
4. Practice using Shodan filters:
   - country:US
   - port:22
   - org:"Organization Name"
```

**Step 5: Web Application Reconnaissance**
```bash
# Use builtwith to identify technologies
# Visit: https://builtwith.com/
# Enter target URL to see technology stack

# Use Wappalyzer browser extension
# Install: https://www.wappalyzer.com/
```

**Step 6: Using recon-ng Framework**
```bash
recon-ng
marketplace search         # View available modules
marketplace install fullcontact
modules load recon/domains-hosts/google_site_web
options set SOURCE example.com
run
```

#### Expected Outcomes
- Complete WHOIS and DNS profile of target domain
- List of discovered subdomains
- Email addresses discovered
- Technology stack identification
- Organized documentation of all findings

---

### 📝 Assignment 2

**Part A: Reconnaissance Report (60 points)**
Choose a PUBLIC organization that has a bug bounty program (e.g., from HackerOne or Bugcrowd). Perform ONLY passive reconnaissance (no active scanning). Document:
- Organization's digital footprint
- Discovered subdomains (minimum 10)
- Technology stack identification
- Employee information from LinkedIn (general trends, not individuals)
- Any publicly exposed documents or data

**Part B: Google Dorking Challenge (40 points)**
Create 10 unique Google dorks and explain what each would reveal about a target organization. Test them on your own public information.

**Submission Format:** Professional report format, 5-8 pages.

---

---

## WEEK 3: Scanning Networks

### 📖 Lecture Notes

#### Topics Covered
- Network Scanning Concepts
- Host Discovery (Ping Sweeps)
- Port Scanning Techniques
- Service and OS Detection
- Scanning Tools Beyond Nmap
- Scanning Evasion Techniques
- TCP/IP Fundamentals for Scanning

#### Key Concepts

**The Three-Way Handshake:**
```
SYN →           (Client sends SYN to server)
      ← SYN/ACK (Server responds with SYN-ACK)
ACK →           (Client sends ACK, connection established)
```

**Port States (Nmap):**
- **Open:** Application is actively accepting connections
- **Closed:** No application listening, but host responds
- **Filtered:** Firewall is blocking probes
- **Unfiltered:** Host responds but port state unknown
- **Open|Filtered:** Cannot determine if open or filtered
- **Closed|Filtered:** Cannot determine if closed or filtered

**Port Scanning Types:**

| Scan Type | Nmap Flag | Description | Stealth |
|-----------|-----------|-------------|---------|
| TCP SYN | -sS | Half-open scan, doesn't complete handshake | High |
| TCP Connect | -sT | Full TCP connection | Low |
| UDP | -sU | Scan UDP ports | Medium |
| ACK | -sA | Map firewall rules | Medium |
| Window | -sW | Similar to ACK scan | Medium |
| NULL | -sN | No flags set | High |
| FIN | -sF | FIN flag only | High |
| XMAS | -sX | FIN+PSH+URG flags | High |

**Common Ports to Know:**
```
21 - FTP          22 - SSH           23 - Telnet
25 - SMTP         53 - DNS           80 - HTTP
110 - POP3        135 - MS RPC       139 - NetBIOS
143 - IMAP        443 - HTTPS        445 - SMB
1433 - MSSQL      1521 - Oracle      3306 - MySQL
3389 - RDP        5432 - PostgreSQL  5900 - VNC
8080 - HTTP Alt   8443 - HTTPS Alt   27017 - MongoDB
```

#### Nmap Command Essentials
```bash
# Basic scan
nmap <target>

# SYN scan with service detection
nmap -sS -sV <target>

# Aggressive scan (OS, services, scripts, traceroute)
nmap -A <target>

# Scan specific ports
nmap -p 80,443,8080 <target>

# Scan port range
nmap -p 1-1000 <target>

# Scan all ports
nmap -p- <target>

# Host discovery only
nmap -sn <target>

# Output to file
nmap -oN output.txt <target>
nmap -oX output.xml <target>
nmap -oA allformats <target>

# Timing templates (T0=sneaky, T5=aggressive)
nmap -T4 <target>

# Evasion techniques
nmap -f <target>                  # Fragment packets
nmap -D RND:10 <target>           # Decoy scanning
nmap --source-port 53 <target>    # Use common source port
nmap --data-length 40 <target>    # Append random data
```

---

### 🔧 Practical Lab 3: Network Scanning with Nmap

#### Objectives
- Master Nmap scanning techniques
- Identify all open ports on Metasploitable
- Detect running services and versions
- Practice scanning evasion techniques
- Analyze scan results

#### Step-by-Step Instructions

**Step 1: Host Discovery**
```bash
# Find live hosts on your lab network
nmap -sn 192.168.56.0/24

# Alternative: ARP ping (works on local network)
sudo nmap -PR -sn 192.168.56.0/24
```

**Step 2: Basic Port Scan**
```bash
# Quick scan of top 1000 ports
nmap 192.168.56.X  # Replace with Metasploitable IP

# Full port scan
nmap -p- 192.168.56.X
```

**Step 3: Service and Version Detection**
```bash
# SYN scan with service version detection
sudo nmap -sS -sV 192.168.56.X

# Aggressive scan
nmap -A 192.168.56.X

# Comprehensive scan
sudo nmap -sS -sV -O -sC -p- -T4 192.168.56.X -oN full_scan.txt
```

**Step 4: UDP Scanning**
```bash
# UDP scan (slow, be patient)
sudo nmap -sU --top-ports 100 192.168.56.X
```

**Step 5: Nmap Scripts (NSE)**
```bash
# List available scripts
ls /usr/share/nmap/scripts/

# Run default scripts
nmap -sC 192.168.56.X

# Run specific scripts
nmap --script vuln 192.168.56.X
nmap --script smb-os-discovery 192.168.56.X
nmap --script ftp-anon 192.168.56.X
```

**Step 6: Zenmap (GUI)**
```bash
# Launch Zenmap for visual interface
zenmap

# Create topology map of your lab network
# Compare scan results over time
```

#### Expected Outcomes
- Complete port listing for Metasploitable
- Service version information
- OS detection results
- Nmap script output
- Saved scan results in multiple formats

---

### 📝 Assignment 3

**Part A: Scanning Analysis (50 points)**
1. Scan Metasploitable with the following scans and document differences in results:
   - TCP SYN scan (-sS)
   - TCP Connect scan (-sT)
   - UDP scan (-sU)
   - NULL scan (-sN)
   - FIN scan (-sF)

2. Why do different scan types produce different results?

**Part B: Nmap Challenge (50 points)**
1. Create an Nmap scan that discovers ALL services on Metasploitable
2. Identify at least 15 open ports
3. For each open port, document:
   - Port number and protocol
   - Service name and version
   - Potential vulnerabilities (research each service)
4. Save results in all Nmap output formats

**Submission Format:** Document with screenshots, scan outputs, and analysis.

---

---

## WEEK 4: Enumeration

### 📖 Lecture Notes

#### Topics Covered
- Enumeration Concepts
- NetBIOS/SMB Enumeration
- SNMP Enumeration
- LDAP Enumeration
- NTP Enumeration
- SMTP/POP3/IMAP Enumeration
- NFS Enumeration
- RPC/DCERPC Enumeration
- DNS Zone Transfers

#### Key Concepts

**Enumeration Defined:**
Enumeration is the process of extracting detailed information about network resources, user accounts, machine names, network shares, and services. This phase builds on scanning results.

**What Enumeration Reveals:**
- Valid usernames and groups
- Network shares and permissions
- Password policies
- System information
- Application configurations
- Mail server information

#### NetBIOS Enumeration
```bash
# Nbtscan - discover NetBIOS names
nbtscan 192.168.56.0/24

# Nmap NSE scripts
nmap --script nbstat.nse -p 137 <target>
nmap --script smb-enum-shares.nse -p 139,445 <target>
nmap --script smb-enum-users.nse -p 139,445 <target>
```

#### SNMP Enumeration
```bash
# SNMPwalk with community strings
snmpwalk -c public -v1 <target>
snmpwalk -c private -v1 <target>
snmpwalk -c manager -v1 <target>

# Nmap SNMP scripts
nmap -sU --script snmp-brute <target>
nmap -sU -p 161 --script snmp-sysdescr <target>
```

#### Common SNMP Community Strings
```
public (read-only, default)
private (read-write, default)
manager
community
test
admin
secret
```

#### SMTP Enumeration
```bash
# Connect to SMTP server
nc <target> 25
EHLO example.com
VRFY root       # Verify if user exists
EXPN admins      # Expand mailing list

# Nmap SMTP scripts
nmap --script smtp-enum-users.nse <target>
nmap --script smtp-open-relay.nse <target>
```

#### DNS Zone Transfer
```bash
# Attempt zone transfer
dig @<DNS_Server> <domain> AXFR

# Using nslookup
nslookup
> server <DNS_Server>
> ls -d <domain>

# Using host command
host -l <domain> <DNS_Server>
```

---

### 🔧 Practical Lab 4: Enumeration Techniques

#### Objectives
- Enumerate services on Metasploitable
- Discover user accounts
- Identify network shares
- Attempt DNS zone transfers
- Document enumeration findings

#### Step-by-Step Instructions

**Step 1: SMB Enumeration**
```bash
# Connect to Metasploitable SMB
smbclient -L //<target>/ -N

# Enumerate users
enum4linux -a <target>

# Nmap SMB enumeration
nmap --script smb-enum-users.nse -p 445 <target>
nmap --script smb-enum-shares.nse -p 445 <target>
nmap --script smb-os-discovery.nse -p 445 <target>
```

**Step 2: SNMP Enumeration**
```bash
# SNMPwalk against Metasploitable
snmpwalk -c public -v2c <target>

# Extract specific information
snmpwalk -c public -v2c <target> 1.3.6.1.2.1.25.1.6.0  # System processes
snmpwalk -c public -v2c <target> 1.3.6.1.2.1.25.4.2.1.2 # Running processes
```

**Step 3: FTP Enumeration**
```bash
# Connect to FTP (Metasploitable allows anonymous)
ftp <target>
Name: anonymous
Password: anonymous@

# Explore available files
ls
pwd
```

**Step 4: NFS Enumeration**
```bash
# Show NFS shares
showmount -e <target>

# Mount NFS share
mkdir /tmp/nfs_share
mount -t nfs <target>:/<share> /tmp/nfs_share
ls -la /tmp/nfs_share
```

**Step 5: RPC Enumeration**
```bash
# RPCBind enumeration
rpcinfo -p <target>

# Nmap RPC scripts
nmap -sV --script rpcinfo <target>
```

#### Expected Outcomes
- List of SMB shares and accessible files
- SNMP system information
- Anonymous FTP access confirmed
- NFS shares identified and accessed
- Complete user enumeration results

---

### 📝 Assignment 4

**Part A: Enumeration Exercise (60 points)**
Perform comprehensive enumeration on Metasploitable:
1. Enumerate all SMB shares and document accessible resources
2. Extract system information via SNMP
3. Attempt FTP anonymous login and list available files
4. Check for NFS shares
5. Create a table of all valid usernames discovered

**Part B: Analysis Questions (40 points)**
1. Why is enumeration considered more targeted than scanning?
2. What security risks do default SNMP community strings present?
3. How can DNS zone transfers aid an attacker?
4. What measures can prevent successful enumeration?
5. Explain the difference between scanning and enumeration with examples.

**Submission Format:** Professional report with tables, screenshots, and analysis.

---

---
