# Advanced — Stealth & IDS Evasion (Red-Team)

> Not needed for internal assessments or lab VMs. Parked here for when you get to external / red-team work.

**:: ALWAYS RUN NMAP AS ROOT OR SPECIFY `-sS` ::**

### 1. Find out how strong their IDS is

**1)** Public job postings (e.g. hiring SOC analysts)

**2)** Public DNS records

**3)** Shodan / Censys public scans

**4)** Rent a cheap VM and run a test scan — see how quickly they ban you (burner test)

### 2. Scan depending on how strong the IDS is

**1) Weak / Basic IDS** (burner test survived, or older infrastructure detected)
- Packet fragmentation (`-f`)
- Decoy scan (`-D`)

**2) Moderate IDS** (burner caught after a volume/speed threshold, or standard commercial firewalls without a dedicated SOC)
- Low and slow (`-T0` or `-T1`)
- Proxychains

**3) Strict IDS** (burner instantly banned on the first packets, or enterprise WAFs like Cloudflare/Imperva + active SOC monitoring)
- Idle scan (`-sI`)

**4) Impenetrable** (active scanning completely impossible, or zero-trust architecture with no public ports)
- Shodan / Censys (passive recon)
