### Target Name: PickleRick

### Target IP: `inconsistent (different TryHackMe sessions)`

### Attacker IP: `inconsistent (different TryHackMe sessions)`

### Difficulty: Easy

# -- Scanning & Enumeration --

# Open Ports

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```

# Operating System & Service Versions (+ additional info)

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-16 15:29 -0400
Nmap scan report for 10.113.162.213
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a2:dd:6a:ef:89:51:37:ee:5d:7e:cf:15:59:82:f1:b5 (RSA)
|   256 e4:9d:eb:eb:d6:39:7c:b7:91:38:9f:ba:ee:af:5f:32 (ECDSA)
|_  256 a2:90:be:7c:c5:e4:0a:29:ce:71:5d:53:d8:3b:f6:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Rick is sup4r cool
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.82 seconds
```

# HTTP (80/TCP)

## via Firefox (+ FFUF)

#### Default Page

![](../../../assets/Pasted%20image%2020260918003035.png)

#### Username hidden in source code: `R1ckRul3s`

![](../../../assets/Pasted%20image%2020260918003156.png)

#### Login Page under `var/www/html/login.php`

![](../../../assets/Pasted%20image%2020260918003808.png)

#### Weird text under `var/www/html/robots.txt`

![](../../../assets/Pasted%20image%2020260918003902.png)

---

# -- Exploitation --

## via Firefox

#### Using the found username and the weird text as a password

![](../../../assets/Pasted%20image%2020260918004116.png)
##### (successful)

#### Access to a command line through `var/www/html/portal.php`

![](../../../assets/Pasted%20image%2020260918004331.png)

#### `cat` disabled

![](../../../assets/Pasted%20image%2020260918004619.png)

#### Found a way to read a file through bash code without `cat` after a bit of googling

![](../../../assets/Pasted%20image%2020260918004828.png)

#### Obtained the first flag: `mr. meeseek hair`

![](../../../assets/Pasted%20image%2020260918004942.png)

#### `/var/www/html/clue.txt` says to explore the file system

![](../../../assets/Pasted%20image%2020260918005104.png)

#### Found out the server has python3 installed

![](../../../assets/Pasted%20image%2020260918005514.png)

#### Obtained shell using a python reverse shell one-liner

![](../../../assets/Pasted%20image%2020260918005745.png)
##### instantly upgraded to an interactive TTY shell

#### obtained the second flag: `1 jerry tear`

![](../../../assets/Pasted%20image%2020260918010112.png)

---

# -- Post-Exploitation --

## Privilege Escalation

### PrivEsc Vectors Investigation

#### ran `sudo -l`; all commands are executable with `sudo` (no password)

![](../../../assets/Pasted%20image%2020260918010205.png)

### Applying found vectors

#### ran `sudo su`; got `root`

![](../../../assets/Pasted%20image%2020260918010255.png)

#### Obtained the third (last) flag: `fleeb juice`

![](../../../assets/Pasted%20image%2020260918010358.png)

---
##### Probably the most satisfying machine I've done so far
