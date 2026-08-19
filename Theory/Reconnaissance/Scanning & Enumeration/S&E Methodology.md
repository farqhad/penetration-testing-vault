# Overall Scanning

### Lessons from failures:

**1) WORDLISTS ARE IMPORTANT. DEEPER ENUMERATION IS IMPORTANT. time wasted: 2 days**

**2) ...**

### Time Management Difficulties?

**1) Run a quick & dirty, shallow scan**

**2) Run a more thorough, deep scan**

**3) Investigate the results of the first scan while the second one is running**

### Service Version Difficulties?

**If you didn't get the service version from the initial scans, try MSFCONSOLE auxiliary modules, service version is everything**

**If that didn't work, try manually with Wireshark (Session Setup AndX Response shows it most of the time)**

### Wanna be sneaky? (Red Team Assessment)

**:: ALWAYS RUN NMAP AS ROOT OR SPECIFY `-sS` ::**

**1. Find out how strong their IDS is**

	1) Public Job Postings (for example hiring SOC Analysts)

	2) Public DNS Records

	3) Shodan/Censys Public Scans

	4) Rent a Cheap VM and run a test scan, seeing how quick they ban you (Burner Test)

**2. Scan depending on how strong the IDS is**

	1) Weak/Basic IDS (Burner test survived or older infrastructure detected)

		- Packet Fragmentation (-f)

		- Decoy Scan (-D)

	2) Moderate IDS (Burner test caught after a volume/speed threshold is met or standard commercial firewalls without a dedicated SOC detected)

		- Low and Slow (-T0 or -T1)

		- Proxychains
		
	3) Strict IDS (Burner test instantly banned on the very first packets or enterprise WAFs like Cloudflare/Imperva and active SOC monitoring detected)

		- Idle Scan (-sI)

	4) Impenetrable (Active scanning is completely impossible or Zero-Trust architecture with no public ports detected)

		- Shodan / Censys (Passive Recon)

### Typical errors for older, Legacy machines

**Your Checklist for Identifying Legacy Errors:**

- `kex error` **(Key Exchange)**
    
- `no match for method`
    
- `protocol negotiation failed`
    
- `SMB1 disabled` **or** `NT_STATUS_CONNECTION_RESET`
    
- `SSL_ERROR_UNSUPPORTED_VERSION`

### Compatibility errors with legacy machines?

**1) kali-tweaks -> Hardening -> allow Wide Compatibility**

### Kali-Tweaks didn't help? (Legacy machines)

***SSH / Hydra Manual Legacy Flags:***

**If the server rejects modern ciphers, force the connection by appending these options to your ssh or hydra command:

`-oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`

**(Example:** `ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc user@IP`**)**

***SMBv1 / Legacy SMB Overrides:***

**Modern smbclient disables SMBv1 (NT1) by default. If a Windows XP/2003 machine throws NT_STATUS_CONNECTION_RESET, force SMBv1:

`smbclient -L //IP/ --option='client min protocol=NT1'`

***HTTPS / Legacy SSL Web Servers:***

**If curl or web tools throw SSL_ERROR_UNSUPPORTED_VERSION against an ancient web server, force the TLS downgrade and lower the security level:

`curl -k --tlsv1.0 --ciphers DEFAULT@SECLEVEL=0 https://IP`

# Checklists (gonna update soon)

### 443/80 (HTTPS/HTTP)

**1) Run WhatWeb or Wappalyzer**

**2) Run Nikto**

**3) Connect through browser (default web page)**

**4) View default web page source code**

**5) Extension fuzzing to determine technology (if needed)**

**6) Directory/Extension fuzzing**

**7) Manual directory investigation**

**8) Burpsuite intercept; investigate response; try repeater (other requests)**

### 139/445 (SMB)

**1) Run nmap to grab the exact OS and SMB version.**

**2) Run enum4linux to blindly scrape users, shares, and password policies.**

**3) Run nmap to check for immediate critical vulnerabilities (EternalBlue, SMBGhost).**

**4) Attempt anonymous manual login to discovered shares using `smbclient //192.168.X.X/sharename (-N)` or `rpcclient`**

**5) If allowed, list files and investigate**

**5) If allowed, recursively download all files and investigate locally. (`prompt OFF`; `recurse ON`; `mget *`)**
