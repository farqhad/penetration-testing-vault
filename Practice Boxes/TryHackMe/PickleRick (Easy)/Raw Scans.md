# nmap_raw

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

# ffuf_raw

```
┌──(kali㉿kali)-[~]
└─$ ffuf -c -ic -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak,.zip -u http://10.112.191.189/FUZZ -fc 404

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.112.191.189/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Extensions       : .php .txt .html .bak .zip 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 404
________________________________________________

index.html              [Status: 200, Size: 1062, Words: 148, Lines: 38, Duration: 425ms]
.html                   [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 1719ms]
.php                    [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 3895ms]
                        [Status: 200, Size: 1062, Words: 148, Lines: 38, Duration: 4487ms]
login.php               [Status: 200, Size: 882, Words: 89, Lines: 26, Duration: 619ms]
assets                  [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 236ms]
portal.php              [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 166ms]
robots.txt              [Status: 200, Size: 17, Words: 1, Lines: 2, Duration: 147ms]
```