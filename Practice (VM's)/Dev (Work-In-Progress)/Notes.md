### Target Name: Dev

### Target IP: 192.168.57.8

### Attacker IP: 192.168.57.4

# -- Scanning & Enumeration --

# Open Ports

```
┌──(root㉿kali)-[~]
└─# nmap -T4 -p- 192.168.57.8
...
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
111/tcp   open  rpcbind
2049/tcp  open  nfs
8080/tcp  open  http-proxy
33155/tcp open  unknown
36849/tcp open  unknown
37197/tcp open  unknown
51149/tcp open  unknown
```

# Operating System & Service Versions (+ additional info)

```
┌──(root㉿kali)-[~]
└─# nmap -T4 -sV -sC -p 22,80,111,2049,8080,33155,36849,37197,51149 192.168.57.8
...
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
...
80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
	|_http-server-header: Apache/2.4.38 (Debian)
	|_http-title: Bolt - Installation error
...
111/tcp   open  rpcbind  2-4 (RPC #100000)
...
2049/tcp  open  nfs      3-4 (RPC #100003)
8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
	| http-open-proxy: Potentially OPEN proxy.
	|_Methods supported:CONNECTION
	|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
	|_http-server-header: Apache/2.4.38 (Debian)
...
33155/tcp open  mountd   1-3 (RPC #100005)
36849/tcp open  nlockmgr 1-4 (RPC #100021)
37197/tcp open  mountd   1-3 (RPC #100005)
51149/tcp open  mountd   1-3 (RPC #100005)
...
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (80/TCP; 8080/TCP)

### via NMAP

```
┌──(root㉿kali)-[/home/kali]
└─# nmap -T4 -sV -sC -p 22,80,111,2049,8080,33155,36849,37197,51149 192.168.57.8
...
80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
	|_http-server-header: Apache/2.4.38 (Debian)
	|_http-title: Bolt - Installation error
...
8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.38 (Debian)
```

### via Browser (+ FFUF)
#### :80

##### Default Page

![Screenshot 2026-08-19 234955](Screenshot%202026-08-19%20234955.png)

##### Information Disclosure

![Pasted image 20260820002140](Pasted%20image%2020260820002140.png)

##### Information Disclosure

![Pasted image 20260820002202](Pasted%20image%2020260820002202.png)

##### Openly available internal server files

![Pasted image 20260820002552](Pasted%20image%2020260820002552.png)

##### Internal server error when trying to open CustomisationExtension.php

![Screenshot 2026-08-20 002606](Screenshot%202026-08-20%20002606.png)

##### Openly available /app 

![Pasted image 20260820004256](Pasted%20image%2020260820004256.png)

##### Credentials disclosed in /app/config/config.yml | DB on sqlite

![Pasted image 20260820003503](Pasted%20image%2020260820003503.png)

##### Permission hierarchy information in /app/config/permissions.yml

![Pasted image 20260820004016](Pasted%20image%2020260820004016.png)

##### Credentials and full configuration in /app/cache/config-cache.json

![Screenshot 2026-08-20 004400](Screenshot%202026-08-20%20004400.png)


#### :8080

![Screenshot 2026-08-19 235305](Screenshot%202026-08-19%20235305.png)

##### BoltWire Page under /dev/

![Pasted image 20260820011143](Pasted%20image%2020260820011143.png)

##### User credentials under /dev/pages/

![Pasted image 20260820011309](Pasted%20image%2020260820011309.png)

##### Administrator credentials in /dev/pages/member.admin

![Pasted image 20260820011431](Pasted%20image%2020260820011431.png)

##### Information Disclosure (when accessed admin account)

![Pasted image 20260820011618](Pasted%20image%2020260820011618.png)

##### Restricted file upload capabilities as an admin or editor

![](assets/Pasted image 20260820013333.png]]

![Screenshot 2026-08-20 013355](Screenshot%202026-08-20%20013355.png)

##### Restrictions for uploading listed in Config section

![Screenshot 2026-08-20 013725](Screenshot%202026-08-20%20013725.png)

### via BurpSuite

#### :80
![Pasted image 20260820000851](Pasted%20image%2020260820000851.png)

#### :8080

![Pasted image 20260820000626](Pasted%20image%2020260820000626.png)

---

# -- Exploitation --

### via TOOL

```
┌──(root㉿kali)-[/home/kali]
└─# command if needed
```

#### Info: ...

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

