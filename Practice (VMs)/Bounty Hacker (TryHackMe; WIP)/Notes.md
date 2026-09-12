### Target Name: Bounty Hacker

### Target IP: `inconsistent (different TryHackMe sessions)`

### Attacker IP: 192.168.133.131

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

# SERVICE (1337/TCP)

## via TOOL

```
┌──(root㉿kali)-[/home/kali]
└─# command if needed
```

```
paste text or screenshot
```

---

# -- Version Research (Potential Vulnerabilities) --

## via TOOL

```
┌──(root㉿kali)-[/home/kali]
└─# command if needed
```

```
paste text or screenshot
```

---

# -- Exploitation --

## via TOOL

```
┌──(root㉿kali)-[/home/kali]
└─# command if needed
```

### Info: ...

```
paste text or screenshot
```

---

# -- Post-Exploitation --

## Privilege Escalation

### PrivEsc Vectors Investigation

#### ...

### Applying found vectors

#### ...

---
##### NOTE IF NEEDED
