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
- [x] Note the protocol version: `1.x` / `1.99` means legacy SSHv1 support → old box, expect kex/cipher issues
- Can't connect (kex / cipher errors)? → see [Legacy Machine Compatibility](Legacy%20Machine%20Compatibility.md)
- [ ] Record the version; the auth-method check and brute-force viability are handled in the Exploitation phase

### 80 / 443 (HTTP / HTTPS)

- [x] Identify tech + version: `nmap -sV -sC`, then WhatWeb / Wappalyzer (gives the full stack — server, DB, language, JS libraries)
- [x] Open the default page in a browser
- [x] View the page source
- [x] Hit the error pages for version / info leaks — 404 and 403 pages often disclose the exact server version
- [x] Check the response headers (Burp, or `curl -I`) — the `Server` header often reveals the exact version
- [x] Directory / file fuzz — use **`directory-list-2.3-medium`**, NOT small (that's the 2-day lesson):
  `ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak,.zip -u http://<IP>/FUZZ -fc 403,404`
  *(note: `-fc 403,404` in one flag — passing `-fc 403 -fc 404` as two flags makes ffuf only apply the last one)*
- Results thin / box feels empty? Re-run with a **bigger** wordlist before assuming there's nothing there
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
- [ ] `NT_STATUS_CONNECTION_RESET` / SMB1-only box? → see [Legacy Machine Compatibility](Legacy%20Machine%20Compatibility.md)

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
