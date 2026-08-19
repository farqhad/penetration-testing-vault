# Per-Service Checklists

> Run these top to bottom for every open port. When a step breaks, jump to [[#When Stuck →]] at the bottom.

### General (always run first)

- [ ] Full TCP port scan: `nmap -T4 -p- <IP>`
- [ ] Version + default scripts + OS on the open ports: `nmap -T4 -sV -sC -p <ports> <IP>`
- [ ] Write down every service + version — this list feeds the Vulnerability Research phase

### 21 (FTP)

- [ ] `nmap -sV -sC` — the `ftp-anon` script auto-checks anonymous login and lists the root directory
- [ ] Anonymous allowed? Confirm manually: `ftp <IP>` → user `anonymous`, password anything
- [ ] Download everything readable: `wget ftp://anonymous:anonymous@<IP>/<file>` (or `get` / `mget *` inside the session)
- [ ] Read every single file — readable files often hand over creds, internal notes, or hints toward other services
- [ ] Triangulate version via Wireshark, then research that version for known vulns

### 22 (SSH)

- [ ] `nmap -sV -sC` — grab the OpenSSH version + protocol
- [ ] Note the protocol version: `1.x` / `1.99` means legacy SSHv1 support → old box, expect kex/cipher issues
- [ ] Can't connect (kex / cipher errors)? → see [[Legacy Machine Compatibility]]
- [ ] Record the version; the auth-method check and brute-force viability are handled in the Exploitation phase

### 80 / 443 (HTTP / HTTPS)

- [ ] Identify tech + version: `nmap -sV -sC`, then WhatWeb / Wappalyzer (gives the full stack — server, DB, language, JS libraries)
- [ ] Open the default page in a browser
- [ ] View the page source
- [ ] Hit the error pages for version / info leaks — 404 and 403 pages often disclose the exact server version
- [ ] Check the response headers (Burp, or `curl -I`) — the `Server` header often reveals the exact version
- [ ] Directory / file fuzz — use **`directory-list-2.3-medium`**, NOT small (that's the 2-day lesson):
  `ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak,.zip -u http://<IP>/FUZZ -fc 403,404`
  *(note: `-fc 403,404` in one flag — passing `-fc 403 -fc 404` as two flags makes ffuf only apply the last one)*
- [ ] Results thin / box feels empty? Re-run with a **bigger** wordlist before assuming there's nothing there
- [ ] Investigate every path found, manually
- [ ] Found a web app? Pin its exact version (login page, `/README`, `/ChangeLog`) and look up known vulns
- [ ] Watch for open directory listings — they can expose source, SQL dumps, backups, config files
- [ ] Run Nikto
- [ ] Burp: intercept, read the responses, send to Repeater to poke other requests

### 139 / 445 (SMB)

- [ ] Get the SMB version + OS: `nmap -sV -sC -O`. Won't resolve? Cross-check with `msfconsole` → `auxiliary/scanner/smb/smb_version`, then Wireshark
- [ ] `enum4linux <IP>` — users, shares, workgroup, password policy, whether anonymous sessions are allowed
- [ ] Check for critical SMB vulns: `nmap --script "smb-vuln*"` — EternalBlue (MS17-010), SMBGhost
- [ ] Anonymous login to shares: `smbclient -L //<IP>/ -N` to list, then `smbclient //<IP>/<share> -N` (IPC$ often allows anon; C$/ADMIN$ usually don't)
- [ ] Readable share? List and investigate, then pull everything: `prompt OFF`, `recurse ON`, `mget *`
- [ ] `NT_STATUS_CONNECTION_RESET` / SMB1-only box? → see [[Legacy Machine Compatibility]]