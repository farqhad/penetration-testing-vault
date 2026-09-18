
> Run these the moment you land a shell.

### Situational awareness (first thing on every shell)

- [ ] `whoami` / `id` - who am I, what groups am I in
- [ ] `hostname`, `ip a` / `ifconfig` - where am I
- [ ] `route` / `ip route` - what we're routing through
- [ ] `arp -a` / `ip neigh` - stale and reachable addresses in the network
- [ ] `netstat -ano` - open ports and existing connections
- [ ] Grab proof of access now (`id` + `ip a` / `ipconfig`) — the thing that's easy to forget at the end
- [ ] Upgrade a raw shell to a proper TTY before doing real work (if using meterpreter, use `shell`)

### Loot the current context

- Look for weak file permissions along the way!
- [ ] `uname -a`, `cat /proc/version`, `cat /etc/issue` for basic system info 
- [ ] `lscpu` for basic cpu info
- [ ] `ps aux` for running tasks
- [ ] `/home/*` - check readable home dirs, `.bash_history`, SSH keys, stray notes
- [ ] `/etc/passwd; /etc/shadow` - which users exist, their password hashes, writeable?
- [ ] `sudo -l` to see what I can execute as root (sudo) without a password

### Vector hunting & application

- [ ] Kernel + OS version → search for a matching kernel exploit
- [ ] Scheduled tasks: `crontab -l` / `systemctl list-timers` / `ps`; or run `pspy` and leave it a while
- [ ] Enumerate credentials with `locate pass/pwd/passwd/password | more` or `grep -rnw '/' -ie "PASSWORD/PWD/PASSWD/PASS/..." --color=always 2> /dev/null | less`
- [ ] Looks for SSH keys: `locate id_rsa | less`; `find / -name authorized_keys 2> /dev/null`; `find / -name id_rsa 2> /dev/null`
- [ ] `find / -perm /4000 2> /dev/null`  to find SUID files
- [ ] try to run `strings /path/to/binary` against an SUID or a SUDO(NOPASSWD) executable file and look for any binaries it runs without specifying the path
-  (you can add dir with a file with the same name to PATH `export PATH=/dir:$PATH` and execute code as this file)
- [ ] App / web config files for hardcoded creds (`config.php`, `.env`, `wp-config.php`, etc.) - DB passwords are routinely reused for SSH and other services. A config file beats an exploit when it's there
- [ ] Every credential you find → try it on every service and every user (password reuse)
- [ ] look through `PayloadsAllTheThings`
- [ ] look through `GTFOBins`
- [ ] Automated sweep: `LinPEAS` / `linux-exploit-suggester` / `LinEnum` / `linuxprivchecker`
- [ ] Try to apply known CVE's
- [ ] Cronjob running as a higher privilege and writable? Drop your shell script in and wait
- [ ] Landed root? `whoami` / `id` to confirm, then grab proof