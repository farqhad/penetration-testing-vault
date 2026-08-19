# Scanning & Enumeration Methodology

## Lessons from failures

**1) WORDLISTS ARE IMPORTANT. DEEPER ENUMERATION IS IMPORTANT. time wasted: 2 days**

**2) SNOW BLINDNESS. In a long list where most entries are noise, read every line — the one that matters hides among the ones that don't. Applies to nmap output, fuzzing results, directory/file listings, enum dumps.**

**3) ALWAYS LOOK FOR CONFIG FILES. During enumeration: exposed over HTTP (backups like `.bak` / `.old` / `.php~`, open directory listings). Post-shell: on-disk for hardcoded creds that get reused elsewhere.**

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

See **[[Legacy Machine Compatibility]]** — how to spot one, and the cipher / SMB / SSL flags to force a connection.

---

*Stealth / IDS evasion (red-team) lives in [[Advanced - Stealth & IDS Evasion]] — not needed for internal or lab work.*
