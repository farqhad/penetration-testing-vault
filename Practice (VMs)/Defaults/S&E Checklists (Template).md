# Per-Service Checklists

## Lessons from failures

> Not steps to tick — hard-won rules to keep in mind while you work.

**1) WORDLISTS ARE IMPORTANT. DEEPER ENUMERATION IS IMPORTANT. time wasted: 2 days**

**2) SNOW BLINDNESS. In a long list where most entries are noise, read every line — the one that matters hides among the ones that don't. Applies to nmap output, fuzzing results, directory/file listings, enum dumps.**

**3) ALWAYS LOOK FOR CONFIG FILES. During enumeration: exposed over HTTP (backups like `.bak` / `.old` / `.php~`, open directory listings). Post-shell: on-disk for hardcoded creds that get reused elsewhere.**

---

> Run these top to bottom for every open port. When a step breaks, jump to [#When Stuck →](#When%20Stuck%20→) at the bottom.

### General (always run first)

- [ ] Full TCP port scan: `nmap -T4 -p- <IP>`
- [ ] Version + default scripts + OS on the open ports: `nmap -T4 -sV -sC -p <ports> <IP>`
- [ ] Write down every service + version — this list feeds the Vulnerability Research phase

### 21 (FTP)

- [ ] Triangulate version via Wireshark, then research that version for known vulnerabilities
- [ ] `nmap -sV -sC` — the `ftp-anon` script auto-checks anonymous login and lists the root directory
- [ ] Anonymous allowed? Confirm manually: `ftp <IP>` → user `anonymous`, password anything
- [ ] Download everything readable: `wget ftp://anonymous:anonymous@<IP>/<file>` (or `get` / `mget *` inside the session)
- [ ] Read every single file — readable files often hand over creds, internal notes, or hints toward other services

### 22 (SSH)

- [ ] `nmap -sV -sC` — grab the OpenSSH version + protocol
- Note the protocol version: `1.x` / `1.99` means legacy SSHv1 support → old box, expect kex/cipher issues
- Can't connect (kex / cipher errors)? → see [Legacy Machine Compatibility](../../Theory/Reference/Legacy%20Machine%20Compatibility.md)

### 80 / 443 (HTTP / HTTPS)

- [ ] Identify tech + version: `nmap -sV -sC`, then WhatWeb / Wappalyzer (gives the full stack — server, DB, language, JS libraries)
- [ ] Open the default page in a browser
- [ ] View the page source
- [ ] Hit the error pages for version / info leaks — 404 and 403 pages often disclose the exact server version
- [ ] Check the response headers (Burp, or `curl -I`) — the `Server` header often reveals the exact version
- [ ] Look up known vulnerabilities
- [ ] Directory / file fuzz — use **`directory-list-2.3-medium`**, NOT small (that's the 2-day lesson):
  `ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak,.zip -u http://<IP>/FUZZ -fc 403,404`
  *(note: `-fc 403,404` in one flag — passing `-fc 403 -fc 404` as two flags makes ffuf only apply the last one)*
- Results thin / box feels empty? Re-run with a **bigger** wordlist before assuming there's nothing there
- [ ] Found a web app? Pin its exact version (login page, `/README`, `/ChangeLog`) 
- [ ] Watch for open directory listings — they can expose source, SQL dumps, backups, config files
- [ ] Second web port (8080 etc.)? Treat it as its own separate app — enumerate it independently
- [ ] Scan flags `http-open-proxy` on a web port? Test if it really forwards: `curl -x http://<IP>:<port> http://example.com`. External page comes back = real open proxy (can tunnel requests, even to the box's own `127.0.0.1` services); nothing back = false positive, drop it
- [ ] Run Nikto
- [ ] Burp: intercept, read the responses, send to Repeater to poke other requests
- [ ] Investigate every path found, manually

### 139 / 445 (SMB)

- [ ] Get the SMB version + OS: `nmap -sV -sC -O`. Won't resolve? Cross-check with `msfconsole` → `auxiliary/scanner/smb/smb_version`, then Wireshark
- [ ] `enum4linux <IP>` — users, shares, workgroup, password policy, whether anonymous sessions are allowed
- [ ] Check for critical SMB vulns: `nmap --script "smb-vuln*"` — EternalBlue (MS17-010), SMBGhost
- [ ] Anonymous login to shares: `smbclient -L //<IP>/ -N` to list, then `smbclient //<IP>/<share> -N` (IPC$ often allows anon; C$/ADMIN$ usually don't)
- [ ] Readable share? List and investigate, then pull everything: `prompt OFF`, `recurse ON`, `mget *`
- `NT_STATUS_CONNECTION_RESET` / SMB1-only box? → see [Legacy Machine Compatibility](../../Theory/Reference/Legacy%20Machine%20Compatibility.md)

### 111 / 2049 (RPC / NFS)

> rpcbind (111) is a switchboard mapping RPC program numbers to ports; NFS (2049) + the random high ports (mountd, nlockmgr) are all one NFS subsystem. Reference, not a step.

- [ ] Note the NFS version from the scan (this comes from the initial `-sV` scan, before anything else) — **v3 present = the weak, trusting model**, which is usually what makes it exploitable
- [ ] Query the switchboard: `rpcinfo <IP>` — dumps every registered RPC program, version, and port (no auth needed). Confirms NFS + mountd are present
- [ ] List the exported shares: `showmount -e <IP>` — THE key NFS command. `*` on the right = any client can mount (jackpot); an IP/range = restricted
- [ ] Mount it: `mkdir /mnt/nfs && sudo mount -t nfs <IP>:/export/path /mnt/nfs` — stubborn? force the weak version with `-o vers=3`
- [ ] Loot the mounted share — creds, SSH keys, configs, anything readable (`cd /mnt/nfs`)
- [ ] Can't read a file (owned by another UID)? NFSv3 trusts whatever UID your client claims, so **become that UID**: on YOUR box (you're root there) `sudo useradd -u <UID> faker`, then `sudo su faker`, then read the file — server sees the matching number and hands it over
- [ ] Export shows `no_root_squash`? Root privesc — the shape: mount as root (your root = real root on the share), copy a shell binary onto it, `sudo chown root:root <bin>` + `sudo chmod u+s <bin>` (SUID bit = runs as its owner root), then execute it from a foothold shell ON the target → root. (Needs a compiled shell binary + an existing foothold; flesh out when you hit a real one)

### 53 (DNS)

> DNS is the phonebook: name ↔ IP. `-d` picks the target domain, `-t` picks the technique. A "nameserver" is the DNS server holding the records; `-n` aims your query at a *specific* one (the target's own, not your default) — required for zone transfers and internal/AD lookups. Reference, not a step.

- [ ] Note the DNS version/software from the initial scan: `nmap -sV -sC -p 53 <IP>`

**Perspective A — you have the IP but NO domain (reverse first):**

- [ ] Reverse-lookup the box's own subnet to turn IPs into hostnames: `dnsrecon -r <IP>/24 -n <IP>` — aims PTR queries at the target's own DNS server. A hit hands you the internal hostname/domain (e.g. `something.tcm`)
- [ ] No `-r` hit? Try a single reverse query straight at the IP to catch a PTR record
- Found a hostname/domain? → do the `/etc/hosts` step below, then pivot to Perspective B to enumerate it further

**Perspective B — you have the domain but NO/partial IP mapping (forward):**

- [ ] Standard first-pass — pull the public records (A, MX, NS, TXT, SOA): `dnsrecon -d <domain> -t std`
- [ ] Try the jackpot — zone transfer dumps the *entire* zone at once if the server is misconfigured: `dnsrecon -d <domain> -t axfr`
- [ ] Zone transfer against a *specific* nameserver you found in the NS records: `dnsrecon -d <domain> -t axfr -n <nameserver>`
- [ ] Zone transfer failed? Bruteforce subdomains from a wordlist: `dnsrecon -d <domain> -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`
- [ ] Suspect Active Directory? Enumerate SRV records to find domain services (LDAP, Kerberos, the DC): `dnsrecon -d <domain> -t srv`

**Always — map the name so the browser/tools can reach it:**

- [ ] Add every domain/subdomain found to `/etc/hosts` so you can browse by name and run normal HTTP/HTTPS enumeration against it: `echo "<IP>  <domain> <subdomain1> <subdomain2>" | sudo tee -a /etc/hosts`
- [ ] Confirm it resolves: `ping -c1 <domain>` (or just load it in the browser)
- Now treat it like any web target → jump to the **80 / 443 (HTTP / HTTPS)** checklist and enumerate (dirbust, subdomains, etc.)

---

# When Stuck →

> Troubleshooting. You don't read this top to bottom — you jump to the relevant one when a checklist step breaks.

### Can't get the service version?

**Try MSFCONSOLE auxiliary modules — service version is everything.**

**Still nothing? Grab it manually in Wireshark (Session Setup AndX Response shows it most of the time).**

### Running low on time?

**1) Run a quick & dirty, shallow scan**

**2) Kick off a more thorough, deep scan**

**3) Investigate the results of the first scan while the second one runs**

### Legacy machine?

See **[Legacy Machine Compatibility](../../Theory/Reference/Legacy%20Machine%20Compatibility.md)** — how to spot one, and the cipher / SMB / SSL flags to force a connection.

---

*Stealth / IDS evasion (red-team) lives in [Advanced - Stealth & IDS Evasion](../../Theory/Reconnaissance/Scanning%20&%20Enumeration/Advanced%20-%20Stealth%20&%20IDS%20Evasion.md) — not needed for internal or lab work.*
