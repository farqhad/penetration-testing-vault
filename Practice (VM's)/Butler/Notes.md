### Target Name: Dev

### Target IP: 192.168.57.8

### Attacker IP: 192.168.57.4

# -- Scanning & Enumeration --

# Open Ports

```
┌──(root㉿kali)-[~]
└─# nmap -T4 -p- 192.168.57.8

PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5040/tcp  open  unknown
8080/tcp  open  http-proxy
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49670/tcp open  unknown
```

# Operating System & Service Versions (+ additional info)

```
┌──(root㉿red-wheelbarrow)-[~]
└─# nmap -T4 -sC -sV -p 135,139,445,5040,8080,49664,49665,49666,49667,49668,49670 192.168.1.15

PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
5040/tcp  open  unknown
8080/tcp  open  http          Jetty 9.4.41.v20210516
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
|_http-server-header: Jetty(9.4.41.v20210516)
| http-robots.txt: 1 disallowed entry 
|_/
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:8F:2E:15 (Oracle VirtualBox virtual NIC)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

# HTTP (8080)

## via NMAP

```
┌──(root㉿red-wheelbarrow)-[~]
└─# nmap -T4 -sC -sV -p 135,139,445,5040,8080,49664,49665,49666,49667,49668,49670 192.168.1.15
...
8080/tcp  open  http          Jetty 9.4.41.v20210516
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
|_http-server-header: Jetty(9.4.41.v20210516)
| http-robots.txt: 1 disallowed entry 
|_/
```

## via Browser (+ FFUF)

### robots.txt page (1 disallowed entry)
![](../../assets/Pasted%20image%2020260825231707.png)