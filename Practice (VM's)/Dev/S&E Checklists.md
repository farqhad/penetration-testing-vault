# Per-Service Checklists

## Lessons from failures

> Not steps to tick — hard-won rules to keep in mind while you work.

**1) WORDLISTS ARE IMPORTANT. DEEPER ENUMERATION IS IMPORTANT. time wasted: 2 days**

**2) SNOW BLINDNESS. In a long list where most entries are noise, read every line — the one that matters hides among the ones that don't. Applies to nmap output, fuzzing results, directory/file listings, enum dumps.**

**3) ALWAYS LOOK FOR CONFIG FILES. During enumeration: exposed over HTTP (backups like `.bak` / `.old` / `.php~`, open directory listings). Post-shell: on-disk for hardcoded creds that get reused elsewhere.**

---

> Run these top to bottom for every open port. When a step breaks, jump to [#When Stuck →](#When%20Stuck%20→) at the bottom.

### General (always run first)

- [x] Full TCP port scan: `nmap -T4 -p- <IP>`
- [x] Version + default scripts + OS on the open ports: `nmap -T4 -sV -sC -p <ports> <IP>`
- [x] Write down every service + version — this list feeds the Vulnerability Research phase

### 22 (SSH)

- [x] `nmap -sV -sC` — grab the OpenSSH version + protocol
- Note the protocol version: `1.x` / `1.99` means legacy SSHv1 support → old box, expect kex/cipher issues
- Can't connect (kex / cipher errors)? → see [Legacy Machine Compatibility](Legacy%20Machine%20Compatibility.md)

### 80 / 443 (HTTP / HTTPS)

- [x] Identify tech + version: `nmap -sV -sC`, then WhatWeb / Wappalyzer (gives the full stack — server, DB, language, JS libraries)
- [x] Open the default page in a browser
- [x] View the page source
- [x] Hit the error pages for version / info leaks — 404 and 403 pages often disclose the exact server version
- [x] Check the response headers (Burp, or `curl -I`) — the `Server` header often reveals the exact version
- [x] Look up known vulnerabilities
- [x] Directory / file fuzz — use **`directory-list-2.3-medium`**, NOT small (that's the 2-day lesson):
  `ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak,.zip -u http://<IP>/FUZZ -fc 403,404`
  *(note: `-fc 403,404` in one flag — passing `-fc 403 -fc 404` as two flags makes ffuf only apply the last one)*
- Results thin / box feels empty? Re-run with a **bigger** wordlist before assuming there's nothing there
- [x] Found a web app? Pin its exact version (login page, `/README`, `/ChangeLog`) 
- [x] Watch for open directory listings — they can expose source, SQL dumps, backups, config files
- [x] Second web port (8080 etc.)? Treat it as its own separate app — enumerate it independently
- [x] Scan flags `http-open-proxy` on a web port? Test if it really forwards: `curl -x http://<IP>:<port> http://example.com`. External page comes back = real open proxy (can tunnel requests, even to the box's own `127.0.0.1` services); nothing back = false positive, drop it
- [x] Run Nikto
- [x] Burp: intercept, read the responses, send to Repeater to poke other requests
- [x] Investigate every path found, manually

### 111 / 2049 (RPC / NFS)

> rpcbind (111) is a switchboard mapping RPC program numbers to ports; NFS (2049) + the random high ports (mountd, nlockmgr) are all one NFS subsystem. Reference, not a step.

- [x] Note the NFS version from the scan (this comes from the initial `-sV` scan, before anything else) — **v3 present = the weak, trusting model**, which is usually what makes it exploitable
- [x] Query the switchboard: `rpcinfo <IP>` — dumps every registered RPC program, version, and port (no auth needed). Confirms NFS + mountd are present
- [x] List the exported shares: `showmount -e <IP>` — THE key NFS command. `*` on the right = any client can mount (jackpot); an IP/range = restricted
- [x] Mount it: `mkdir /mnt/nfs && sudo mount -t nfs <IP>:/export/path /mnt/nfs` — stubborn? force the weak version with `-o vers=3`
- [x] Loot the mounted share — creds, SSH keys, configs, anything readable (`cd /mnt/nfs`)
- Can't read a file (owned by another UID)? NFSv3 trusts whatever UID your client claims, so **become that UID**: on YOUR box (you're root there) `sudo useradd -u <UID> faker`, then `sudo su faker`, then read the file — server sees the matching number and hands it over
- Export shows `no_root_squash`? Root privesc — the shape: mount as root (your root = real root on the share), copy a shell binary onto it, `sudo chown root:root <bin>` + `sudo chmod u+s <bin>` (SUID bit = runs as its owner root), then execute it from a foothold shell ON the target → root. (Needs a compiled shell binary + an existing foothold; flesh out when you hit a real one)

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

See **[Legacy Machine Compatibility](Legacy%20Machine%20Compatibility.md)** — how to spot one, and the cipher / SMB / SSL flags to force a connection.

---

*Stealth / IDS evasion (red-team) lives in [Advanced - Stealth & IDS Evasion](Advanced%20-%20Stealth%20&%20IDS%20Evasion.md) — not needed for internal or lab work.*
