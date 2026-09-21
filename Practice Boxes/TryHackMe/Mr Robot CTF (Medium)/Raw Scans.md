
# nmap_raw

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-18 15:26 -0400
Nmap scan report for 10.113.190.239
Host is up (0.26s latency).

PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 00:db:33:85:95:9d:ee:2c:e4:4e:a4:a1:58:fe:dd:67 (RSA)
|   256 ef:49:d5:a3:22:db:09:78:d9:26:83:67:f8:60:78:87 (ECDSA)
|_  256 06:c8:c5:f0:4c:ca:5b:2b:3b:1e:f7:b4:d4:b6:5f:6c (ED25519)
80/tcp  open  http     Apache httpd
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache
443/tcp open  ssl/http Apache httpd
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Apache
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.94 seconds
```

# ffuf_raw

```
        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.113.190.239/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Extensions       : .php .html .txt .bak .zip
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 404
________________________________________________

.html                   [Status: 403, Size: 214, Words: 16, Lines: 10, Duration: 99ms]
images                  [Status: 301, Size: 237, Words: 14, Lines: 8, Duration: 101ms]
index.html              [Status: 200, Size: 1188, Words: 189, Lines: 31, Duration: 102ms]
                        [Status: 200, Size: 1188, Words: 189, Lines: 31, Duration: 104ms]
index.php               [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 523ms]
blog                    [Status: 301, Size: 235, Words: 14, Lines: 8, Duration: 57ms]
rss                     [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 79ms]
sitemap                 [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 54ms]
login                   [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 616ms]
0                       [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 306ms]
feed                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 267ms]
video                   [Status: 301, Size: 236, Words: 14, Lines: 8, Duration: 65ms]
image                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 615ms]
atom                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 271ms]
wp-content              [Status: 301, Size: 241, Words: 14, Lines: 8, Duration: 42ms]
admin                   [Status: 301, Size: 236, Words: 14, Lines: 8, Duration: 43ms]
audio                   [Status: 301, Size: 236, Words: 14, Lines: 8, Duration: 121ms]
intro                   [Status: 200, Size: 516314, Words: 2076, Lines: 2028, Duration: 84ms]
wp-login                [Status: 200, Size: 2620, Words: 115, Lines: 53, Duration: 861ms]
wp-login.php            [Status: 200, Size: 2620, Words: 115, Lines: 53, Duration: 1082ms]
css                     [Status: 301, Size: 234, Words: 14, Lines: 8, Duration: 69ms]
rss2                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 559ms]
license                 [Status: 200, Size: 309, Words: 25, Lines: 157, Duration: 309ms]
license.txt             [Status: 200, Size: 309, Words: 25, Lines: 157, Duration: 309ms]
wp-includes             [Status: 301, Size: 242, Words: 14, Lines: 8, Duration: 37ms]
js                      [Status: 301, Size: 233, Words: 14, Lines: 8, Duration: 40ms]
wp-register.php         [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 485ms]
Image                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 345ms]
wp-rss2.php             [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 465ms]
rdf                     [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 307ms]
page1                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 198ms]
readme                  [Status: 200, Size: 64, Words: 14, Lines: 2, Duration: 63ms]
readme.html             [Status: 200, Size: 64, Words: 14, Lines: 2, Duration: 35ms]
robots                  [Status: 200, Size: 41, Words: 2, Lines: 4, Duration: 58ms]
robots.txt              [Status: 200, Size: 41, Words: 2, Lines: 4, Duration: 57ms]
dashboard               [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 563ms]
%20                     [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 283ms]
wp-admin                [Status: 301, Size: 239, Words: 14, Lines: 8, Duration: 59ms]
phpmyadmin              [Status: 403, Size: 94, Words: 14, Lines: 1, Duration: 47ms]
0000                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 477ms]
wp-atom.php             [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 639ms]
wp-commentsrss2.php     [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 670ms]
xmlrpc                  [Status: 405, Size: 42, Words: 6, Lines: 1, Duration: 793ms]
xmlrpc.php              [Status: 405, Size: 42, Words: 6, Lines: 1, Duration: 858ms]
wp-rdf.php              [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 538ms]
```