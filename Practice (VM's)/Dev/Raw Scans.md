# nmap_raw

```
┌──(root㉿kali)-[~]
└─# nmap -T4 -sV -sC -p 22,80,111,2049,8080,33155,36849,37197,51149 192.168.57.8 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-19 07:25 -0400
Nmap scan report for 192.168.57.8
Host is up (0.00096s latency).

PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 bd:96:ec:08:2f:b1:ea:06:ca:fc:46:8a:7e:8a:e3:55 (RSA)
|   256 56:32:3b:9f:48:2d:e0:7e:1b:df:20:f8:03:60:56:5e (ECDSA)
|_  256 95:dd:20:ee:6f:01:b6:e1:43:2e:3c:f4:38:03:5b:36 (ED25519)
80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Bolt - Installation error
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      33155/tcp   mountd
|   100005  1,2,3      42117/udp6  mountd
|   100005  1,2,3      46878/udp   mountd
|   100005  1,2,3      49975/tcp6  mountd
|   100021  1,3,4      36295/tcp6  nlockmgr
|   100021  1,3,4      36849/tcp   nlockmgr
|   100021  1,3,4      37466/udp6  nlockmgr
|   100021  1,3,4      49642/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs      3-4 (RPC #100003)
8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.38 (Debian)
33155/tcp open  mountd   1-3 (RPC #100005)
36849/tcp open  nlockmgr 1-4 (RPC #100021)
37197/tcp open  mountd   1-3 (RPC #100005)
51149/tcp open  mountd   1-3 (RPC #100005)
MAC Address: 08:00:27:E4:C4:E1 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.89 seconds
```
