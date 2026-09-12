
> Run these the moment you land a shell. '()' in commands usually means optional appendix.

### Situational awareness (first thing on every shell)

- [ ] Grab proof of access now - `whoami`; `ipconfig `(`/all`)
- [ ] `arp -a` for other devices' information
- [ ] `netstat -ano` for open ports' information
- [ ] Upgrade the shell to a proper one before doing real work (if using meterpreter, use `shell`)

### Loot the current context

- [ ] Run `systeminfo `(`| findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"`) for basic system information
- [ ] `wmic qfe` for patches' and hotfixes' information
- [ ] `list drives` or `wmic logicaldisk `(`get caption,description,priovidername`) for logical disk information
- [ ] `whoami /priv` and `whoami /groups` for privileges and groups
- [ ] `net user `(`<username>`) for users' information or a particular user's information
- [ ] `net localgroup `(`<groupname>`) for groups' information or a particular group's information
- [ ] `sc query windefend` for windows defender information
- [ ] `sc queryex type= service` for all services' extended information
- [ ] `netsh advfirewall firewall dump` or `netsh firewall show state` for firewall information
- [ ] `netsh firewall show config` for firewall configuration
- [ ] Every credential you find → try it on every service and every user (password reuse)

### Privilege escalation — vector hunting

- [ ] look for password logs `findstr /si password *.txt `(`*.ini *.config *...`)
- [ ] try to pull the wlan password and reuse
- [ ] look through `HackTricks' Windows PrivEsc Checklist`
- [ ] look through `PayloadsAllTheThings`
- [ ] look through `LOLBAS`
- [ ] Automated sweep: `winPEAS`

### Applying a vector

- [ ] Try to apply known CVE's
- [ ] Transfer the exploit to the target
- [ ] Landed SYSTEM/admin? Confirm, then grab proof