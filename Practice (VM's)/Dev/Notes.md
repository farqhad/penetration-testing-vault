### Target Name: Dev

### Target IP: 192.168.57.8

### Attacker IP: 192.168.57.4

# -- Scanning & Enumeration --

**[x] Read [S&E Methodology](../../Theory/Reconnaissance/Scanning%20&%20Enumeration/S&E%20Methodology.md)**

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

#### Debian 10; Linux Kernel 4.15-5.19

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

# HTTP (80/TCP)

# -- Exploitation --

**[ ] Read [Exploitation Methodology](../../Theory/Exploitation/Exploitation%20Methodology.md)**

# -- Post-Exploitation --

**[ ] Read [Post-Exploitation Methodology](../../Theory/Post-Exploitation/Post-Exploitation%20Methodology.md)**
