### Target Name: Academy
### Target IP: 192.168.57.6
### Attacker IP: 192.168.57.4
# -- Scanning & Enumeration --
# Open Ports
```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -p- 192.168.57.6
...
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```
# Operating System
#### Debian 10; Linux Kernel 4.15-5.19
```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -sV -sC -O -p 21,22,80 192.168.57.6
...
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
...
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
...
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
# FTP (21/TCP)
### via NMAP
```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -sV -sC -O -p 21,22,80 192.168.57.6
```
#### Client&Version: vsftpd 3.0.3
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
```
#### Anonymous Login: allowed + access to note.txt file
```
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1000     1000          776 May 30  2021 note.txt
```
#### Server Status Info:
```
| FTP server status:
|      Connected to ::ffff:192.168.57.4
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
```
### via CLI
#### Client&Version: vsftpd 3.0.3
#### Anonymous Login: allowed + access to note.txt file
```
┌──(root㉿kali)-[/home/kali]
└─# ftp 192.168.57.6 -P 21
Connected to 192.168.57.6.
220 (vsFTPd 3.0.3)
Name (192.168.57.6:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```
#### note.txt file: downloaded via `wget`
```
┌──(kali㉿kali)-[~]
└─$ wget ftp://anonymous:anonymous@192.168.57.6/note.txt
--2026-08-16 08:56:09--  ftp://anonymous:*password*@192.168.57.6/note.txt
           => ‘note.txt’
Connecting to 192.168.57.6:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD not needed.
==> SIZE note.txt ... 776
==> PASV ... done.    ==> RETR note.txt ... done.
Length: 776 (unauthoritative)

note.txt                             100%[===================================================================>]     776  --.-KB/s    in 0.002s  

2026-08-16 08:56:09 (307 KB/s) - ‘note.txt’ saved [776]
```
#### note.txt file: directly reveals SQL Injection possibility + an existing student's credentials
```
┌──(kali㉿kali)-[~]
└─$ cat note.txt 
Hello Heath !
Grimmie has setup the test website for the new academy.
I told him not to use the same password everywhere, he will change it ASAP.

I couldn't create a user via the admin panel, so instead I inserted directly into the database with the following command:

INSERT INTO `students` (`StudentRegno`, `studentPhoto`, `password`, `studentName`, `pincode`, `session`, `department`, `semester`, `cgpa`, `creationdate`, `updationDate`) VALUES
('10201321', '', 'cd73502828457d15655bbd7a63fb0bc8', 'Rum Ham', '777777', '', '', '', '7.60', '2021-05-29 14:36:56', '');

The StudentRegno number is what you use for login.

Le me know what you think of this open-source project, it's from 2020 so it should be secure... right ?
We can always adapt it to our needs.

-jdelta
```
### via Wireshark
#### Client&Version: vsftpd 3.0.3
![[Pasted image 20260816145048.png]]
# HTTP (80/TCP)
### via NMAP
```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -sV -sC -O -p 21,22,80 192.168.57.6
```
#### Client&Version: Apache httpd 2.4.38 ((Debian))
```
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
```
#### Default Page: Apache
```
|_http-title: Apache2 Debian Default Page: It works
```
### via Browser (+ FFUF)
#### Technology: Apache (Default Page)
![[Pasted image 20260816151331.png]]
#### Version: Apache/2.4.38 (404 Page)
![[Pasted image 20260816151539.png]]
#### Version: Apache/2.4.38 (403 Page)
![[Pasted image 20260816172115.png]]
#### Technology: MySQL (PhpMyAdmin Login Page)
![[Pasted image 20260816173145.png]]
#### Back-End Function Used: mysqli_real_connect(); (PhpMyAdmin Login Page)
![[Pasted image 20260816173723.png]]
#### Explicitly stated authentication token in the PhpMyAdmin Login Page source code
![[Pasted image 20260817160549.png]]
#### Version: PhpMyAdmin 4.9.7 (PhpMyAdmin README Page)
![[Pasted image 20260816181837.png]]
#### Fixed Vulnerabilities & Version: broken 2FA; PMASA-2020-1,2,3,4,5,6 & PhpMyAdmin 4.9.7 (PhpMyAdmin ChangeLog)
![[Pasted image 20260816182143.png]]
##### Note: PhpMyAdmin 4.9.7 is compatible with MySQL 5.5 or newer
![[Pasted image 20260817155619.png]]
#### Public Access to DbSeed under /academy/db
![[Screenshot 2026-08-17 221048.png]]
### via BurpSuite
#### Version: Apache/2.4.38 (Debian) & HTTP/1.1
![[Pasted image 20260816152555.png]]
### via Wappalyzer
#### Services&Versions
![[Pasted image 20260817145807.png]]
# SSH (22/TCP)
### via NMAP
```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -sV -sC -O -p 21,22,80 192.168.57.6
```
#### Client&Version: OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
```
### via CLI
#### Password-based authentication (potential for brute-force):
```
┌──(root㉿kali)-[~]
└─# ssh 192.168.57.6 
...
debug1: Authentications that can continue: publickey,password
...
debug1: Next authentication method: password
root@192.168.57.6's password: 
```

# -- Vulnerability Research (Potential Vulnerabilities) --
### via hints & common sense
#### SQL Injection hint in the note.txt file (FTP Anonymous Login)
### via SearchSploit
#### PhpMyAdmin
```
phpMyAdmin - '/scripts/setup.php' PHP Code Injection

phpMyAdmin - 'pmaPWN!' Code Injection / Remote Code Execution

phpMyAdmin - 'preg_replace' (Authenticated) Remote Code Execution (Metasploit)

phpMyAdmin - 'tbl_gis_visualization.php' Multiple Cross-Site Scripting Vulnerabilities

phpMyAdmin - (Authenticated) Remote Code Execution (Metasploit)

phpMyAdmin - Client-Side Code Injection / Redirect Link Falsification

phpMyAdmin - Config File Code Injection (Metasploit)
```
#### Apache 2.4.38
```
Apache 2.4.x - Buffer Overflow
```
#### vsftpd 3.0.3
```
vsftpd 3.0.3 - Remote Denial of Service
```
####
### via MSFCONSOLE (Metasploit)
#### PhpMyAdmin: Authenticated Remote Code Execution (3 modules; type:exploit)
### via Google
#### PhpMyAdmin 4.9.7: bypassing 2FA
![[Pasted image 20260817021415.png]]
##### Note: it was previously mentioned in the ChangeLog file that the 4.9.7 version included "fix broken Two-Factor Authentication", so this might've been patched

#### Apache 2.4.38: Heap-based buffer overflow (CVE-2026-34356)
![[Pasted image 20260817022149.png]]
#### Apache 2.4.38: Stack-based buffer overflow (CVE-2026-34355)
![[Screenshot 2026-08-17 022405.png]]
#### vsftpd 3.0.3: remote DoS (CVE-2021-30047)![[Screenshot 2026-08-17 022534.png]]
#### jQuery 3.4.1: XSS Vulnerability
![[Pasted image 20260817152647.png]]
![[Pasted image 20260817152751.png]]
# -- Desperation --
#### Literally nothing is working and I'm desperate.
#### Asked AI for a hint: turns out there's a "/academy" directory that I would've found if I used a larger wordlist (THE ONLY HINT THAT I GOT)
#### Lesson: WORDLISTS ARE IMPORTANT. DEEPER ENUMERATION IS IMPORTANT.
```
┌──(root㉿kali)-[/usr/share/wordlists]
└─# ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -e .php,.txt,.zip,.bak,.html -u http://192.168.57.6:80/FUZZ -fc 403 -fc 404

index.html              [Status: 200, Size: 10701, Words: 3427, Lines: 369]
academy                 [Status: 301, Size: 314, Words: 20, Lines: 10]
phpmyadmin              [Status: 301, Size: 317, Words: 20, Lines: 10]
```
# --
# -- Exploitation --
### via Browser
#### SQL Injection possible on the /academy page
![[Pasted image 20260817214849.png]]

![[Screenshot 2026-08-17 214911.png]]
##### Note: Usage of previously discovered credentials (note.txt) gives the same result (password decoded from an MD5 hash, exists in open databases)
#### SQL Injection also possible on the /academy/admin page
![[Screenshot 2026-08-17 220724.png]]
#### Unrestricted Photo Upload under /academy/my-profile.php
![[Pasted image 20260817222358.png]]
#### -which allowed for a php reverse shell script remote execution
![[Pasted image 20260817222523.png]]
# -- Post-Exploitation --
## Privilege Escalation (Manual)
### Information Gathering
#### Home directory contains user "grimmie"
![[Pasted image 20260818012346.png]]
##### Note: same guy that "uses the same password everywhere"

# -- Desperation(2) --
### Got stuck here, asked AI for another hint on PrivEsc.
#### Lesson(2): If MOST of the listed things don't matter, doesn't mean that ALL of them don't matter. Look closely at each one. Also ALWAYS look for config files.
![[Pasted image 20260818125141.png]]
# --
### Grimmie's password in the `config.php` file
![[Pasted image 20260818130219.png]]
### Which allowed for an ssh connection as grimmie
![[Pasted image 20260818130407.png]]

# The End.
##### Decided to not escalate to 'root' because I have no privilege escalation knowledge and am exhausted. Had the idea to use LinPeas but what's the point of using something you don't fully understand.
