### Target Name: Ignite

### Target IP: 10.114.158.114

### Attacker IP: 192.168.162.245

# -- Scanning & Enumeration --

# Open Ports

```
┌──(root㉿red-wheelbarrow)-[~]
└─# nmap -T4 -p- 10.114.158.114
...
PORT   STATE SERVICE
80/tcp open  http
```

# Operating System & Service Versions (+ additional info)

```
┌──(root㉿red-wheelbarrow)-[~]
└─# nmap -T4 -sC -sV -p 80 10.114.158.114 
...
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-title: Welcome to FUEL CMS
|_http-server-header: Apache/2.4.18 (Ubuntu)
```

# HTTP (80/TCP)

## via Browser (+ FFUF)

### Fuel CMS 1.4; Default Page

![](../../assets/Pasted%20image%2020260828171553.png)

### admin login endpoint apparent without dirbust (potentially working default credentials)

![](../../assets/Pasted%20image%2020260828171929.png)

### working default credentials on admin login page

![](../../assets/Pasted%20image%2020260828172149.png)

---

# -- Version Research (Potential Vulnerabilities) --

## via searchsploit

### Fuel CMS 1.4 potentially vulnerable to RCE and Authenticated SQL Injection

![](../../assets/Screenshot%20From%202026-08-28%2017-23-48.png)

---

# -- Exploitation --

## via searchsploit (-m)

```
┌──(root㉿red-wheelbarrow)-[~]
└─# searchsploit -m php/webapps/50477.py
...
  Exploit: Fuel CMS 1.4.1 - Remote Code Execution (3)
      URL: https://www.exploit-db.com/exploits/50477
     Path: /usr/share/exploitdb/exploits/php/webapps/50477.py
    Codes: CVE-2018-16763
 Verified: False
File Type: Python script, ASCII text executable
Copied to: /root/50477.py
```

### RCE successful

![](../../assets/Screenshot%20From%202026-08-28%2018-24-01.png)

### transferring a PHP reverse shell script

![](../../assets/Pasted%20image%2020260828185837.png)

### running it in the browser & connecting

![](../../assets/Pasted%20image%2020260828190025.png)

![](../../assets/Pasted%20image%2020260828190043.png)

---

# -- Post-Exploitation --

## Privilege Escalation

### PrivEsc Vectors Investigation

#### transferred linPEAS

![](../../assets/Pasted%20image%2020260828191456.png)

#### ...



### Applying found vectors

#### ...

---
