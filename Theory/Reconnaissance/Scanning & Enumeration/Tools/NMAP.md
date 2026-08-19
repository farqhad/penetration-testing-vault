### -sS (stealthScan)

***explanation:*** **breaks the TCP connection by sending an RST in response to the SYN ACK; makes itself undetectable for older, shittier firewalls**

***usage:*** **nmap -sS 192.168.0.1**

### -T<1-5> (Timing)

***explanation:*** **decides the speed of the scan, the slower the more thorough, 1 - slow, 5 - fast**

***usage:*** **nmap -T4 192.168.0.1**

### -p (ports)

***explanation:*** **if left as is, scans only the top 1000 ports, one can also specify certain ports or enforce scanning ALL available ports (see usages respectively)**

***usage(1):*** **nmap -p 192.168.0.1**

***usage(2):*** **nmap -p 443,80 192.168.0.1**

***usage(3):*** **nmap -p- 192.168.0.1**

### -A (All)

***explanation:*** **OS detection, version detection, script scanning, and traceroute all in one. Highly comprehensive but very loud, easily detected by firewalls/IDS.**

***usage(1):*** **nmap -A 192.168.0.1**

### -O (OS Detection)

***explanation:*** **attempts to guess the target's operating system. Largely considered useless because it's slow, noisy, inaccurate, the OS is almost always explicitly revealed anyway.**

***usage:*** **nmap -O 10.10.100.180**

### -oG (outputGrepable)

***explanation:*** **saves results into a line-based text file. Designed to be easily parsed by Linux command-line tools.**

***usage(1):*** **nmap -T4 -p- 192.168.0.1 -oG initial_scan.txt**

### -sU (scanUDP)

***explanation:*** **scans for open UDP ports instead of the default TCP ports. Notoriously slow, often returns ambiguous "open|filtered" results. Requires root privileges to run because it crafts raw packets. Highly recommended to target only common UDP ports (53, 161, or 500).**

***usage(1):*** **sudo nmap -sU 192.168.0.1**

***usage(2):*** **sudo nmap -sU -p 53,67,161,500 192.168.0.1**

### -sC (Default Scripts)

***explanation:*** **runs default set of NSE (Nmap Scripting Engine) scripts against the target. Deeper enumeration, banner grabbing, and checking for basic vulnerabilities.***

***usage:*** **nmap -sC 10.10.100.180**

### -sV (Service Versioning)

***explanation:*** **probes open ports to determine the exact service and software version number running on them (e.g., identifying "Apache 2.4.41" instead of just guessing "HTTP" based on port 80).**

***usage:*** **nmap -sV 10.10.100.180**

### -oN (Output Normal)

***explanation:*** **saves the scan results into a text file in "normal" format, the saved file will look exactly like the human-readable output displayed on your terminal screen.**

***usage:*** **nmap -oN nmap/initial 10.10.100.180**

### -F (Fast Scan)

***explanation:*** **scan only the top 100 common ports instead of a 1000**

***usage:*** **nmap -F 10.10.100.180**

### -Pn (No Ping)

***explanation:*** **skips the initial ICMP ping discovery phase and assumes the target host is alive; prevents the scan from failing if a firewall is configured to drop ping requests**

***usage:*** **nmap -Pn 192.168.0.1**

### -f (Packet Fragmentation)

***explanation:*** **breaks the TCP header into tiny fragments to bypass older or poorly configured intrusion detection systems and firewalls that struggle to reassemble them quickly**

***usage:*** **nmap -f 192.168.0.1**

### -D (Decoy Scan)

***explanation:*** **spoofs multiple fake IP addresses alongside your own during the scan, flooding the target's logs to hide your true IP identity in the noise**

***usage:*** **nmap -D RND:10 192.168.0.1**

### -sI (Idle Scan)

***explanation:*** **bounces spoofed packets off a vulnerable "zombie" machine, determining open ports by mathematically monitoring the zombie's IP ID counter without your IP ever interacting with the target**

***usage:*** **nmap -sI 10.0.0.5 192.168.0.1**

# Common Usage Combos

### The All-In-One (Slow, Loud & Lazy)

**nmap -T4 -p- -A 192.168.0.1 -oN nmap_aio.txt**

### Staged All-In-One Scanning (Quick & Noisy)

**1) nmap -T4 -p- 192.168.0.1 -oG - | grep -oP '\d{1,5}/open' | awk -F '/' '{print $1}' | paste -sd,**

**2) copy the output (for instance 443,80)**

**3) nmap -T4 -p 443,80 -A 192.168.0.1 -oN nmap_staged-aio.txt**

### Staged Targeted Enumeration (Lean & Focused)

**1) nmap -T4 -p- 192.168.0.1 -oG - | grep -oP '\d{1,5}/open' | awk -F '/' '{print $1}' | paste -sd,**

**2) copy the output (for instance 443,80)**

**3) nmap -sC -sV -p 443,80 192.168.0.1 -oN nmap_staged-targeted.txt**

### Staged Lightweight Version Probe (Low Noise)

**1) nmap -T4 -p- 192.168.0.1 -oG - | grep -oP '\d{1,5}/open' | awk -F '/' '{print $1}' | paste -sd,**

**2) copy the output (for instance 443,80)**

**3) nmap -sV -p 443,80 192.168.0.1 -oN nmap_staged-versions.txt**

### Deep Inspection & Script Debugging (Full Trace)

**1) nmap -T4 -p- 192.168.0.1 -oG - | grep -oP '\d{1,5}/open' | awk -F '/' '{print $1}' | paste -sd,**

**2) copy the output (for instance 443,80)**

**3) nmap -sC -sV -O --script-trace -p 443,80 192.168.0.1 -oN nmap_staged-debug.txt**

# Nmap Scripting Engine (NSE)

### Explanation

**Nmap includes hundreds of automated scripts to perform advanced enumeration and vulnerability scanning on discovered ports. Scripts are located in `/usr/share/nmap/scripts/`.**

### Common Usages

**1)** **nmap --script smb-os-discovery -p 139,445 192.168.1.10** *(Extracts the OS, computer name, domain, and exact SMB dialect/version)*

**2)** **nmap --script smb-enum-shares -p 139,445 192.168.1.10** *(Attempts to list all available SMB shares and your access permissions)*

**3)** **nmap --script vuln -p 139,445 192.168.1.10** *(Runs every script categorized as 'vuln' to check for critical flaws like MS17-010 EternalBlue)*
