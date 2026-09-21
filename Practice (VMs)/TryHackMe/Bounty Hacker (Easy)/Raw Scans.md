# nmap_raw

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-12 17:14 -0400
Nmap scan report for 10.113.131.225
Host is up (0.092s latency).

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
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.67 seconds

```