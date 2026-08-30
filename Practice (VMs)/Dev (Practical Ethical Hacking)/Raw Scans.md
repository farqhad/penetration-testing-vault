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

# ffuf_raw_8080

```
┌──(root㉿kali)-[~]
└─# ffuf -c -ic -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.zip,.bak,.html -u http://192.168.57.8:8080/FUZZ -fc 404            

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.57.8:8080/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Extensions       : .php .txt .zip .bak .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 404
________________________________________________

.php                    [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 10ms]
                        [Status: 200, Size: 94607, Words: 4689, Lines: 1160, Duration: 26ms]

index.php               [Status: 200, Size: 94635, Words: 4689, Lines: 1160, Duration: 36ms]

.html                   [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 314ms]

dev                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 2ms]

.php                    [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 19ms]

.html                   [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 18ms]
                        [Status: 200, Size: 94608, Words: 4689, Lines: 1160, Duration: 327ms]

server-status           [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 12ms]

:: Progress: [1323282/1323282] :: Job [1/1] :: 4347 req/sec :: Duration: [0:05:58] :: Errors: 0 ::

```

# ffuf_raw_80

```
┌──(root㉿kali)-[~]
└─# ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.zip,.bak,.html -u http://192.168.57.8:80/FUZZ -fc 404 -fc 403

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.57.8:80/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Extensions       : .php .txt .zip .bak .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 200
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 403
________________________________________________

                        [Status: 200, Size: 3833, Words: 926, Lines: 108, Duration: 17ms]

public                  [Status: 301, Size: 313, Words: 20, Lines: 10, Duration: 4ms]

src                     [Status: 301, Size: 310, Words: 20, Lines: 10, Duration: 2ms]

index.php               [Status: 200, Size: 3833, Words: 926, Lines: 108, Duration: 3383ms]

app                     [Status: 301, Size: 310, Words: 20, Lines: 10, Duration: 2ms]

vendor                  [Status: 301, Size: 313, Words: 20, Lines: 10, Duration: 4ms]

extensions              [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 2ms]
                        [Status: 200, Size: 3833, Words: 926, Lines: 108, Duration: 334ms]
:: Progress: [1323282/1323282] :: Job [1/1] :: 131 req/sec :: Duration: [0:11:09] :: Errors: 55 ::
```

# ffuf_raw_8080_dev

```
┌──(root㉿kali)-[~]
└─# ffuf -c -ic -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.zip,.bak,.html -u http://192.168.57.8:8080/dev/FUZZ -fc 404        

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.57.8:8080/dev/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Extensions       : .php .txt .zip .bak .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 200
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response status: 404
________________________________________________

index.php               [Status: 200, Size: 7627, Words: 843, Lines: 144, Duration: 5ms]

.php                    [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 29ms]
                        [Status: 200, Size: 7627, Words: 843, Lines: 144, Duration: 30ms]
files                   [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 3ms]

pages                   [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 1ms]

forms                   [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 3ms]

.html                   [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 1553ms]

config                  [Status: 301, Size: 324, Words: 20, Lines: 10, Duration: 6ms]

stamps                  [Status: 301, Size: 324, Words: 20, Lines: 10, Duration: 52ms]

.php                    [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 29ms]

.html                   [Status: 403, Size: 279, Words: 20, Lines: 10, Duration: 30ms]
                        [Status: 200, Size: 7627, Words: 843, Lines: 144, Duration: 144ms]
:: Progress: [1323282/1323282] :: Job [1/1] :: 187 req/sec :: Duration: [0:05:48] :: Errors: 0 ::

```

# nikto_raw_80

```
┌──(root㉿kali)-[~]
└─# nikto -h 192.168.57.8     
- Nikto v2.6.0
---------------------------------------------------------------------------
+ Target IP:          192.168.57.8
+ Target Hostname:    192.168.57.8
+ Target Port:        80
+ Platform:           Linux/Unix
+ Start Time:         2026-08-21 12:06:06 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.4.38 (Debian)
+ ERROR: Failed to check for updates: 403
+ No CGI Directories found (use '-C all' to force check all possible dirs). CGI tests skipped.
+ [013587] /: Suggested security header missing: permissions-policy. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Permissions-Policy
+ [013587] /: Suggested security header missing: content-security-policy. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
+ [013587] /: Suggested security header missing: referrer-policy. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy
+ [013587] /: Suggested security header missing: x-content-type-options. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options
+ [013587] /: Suggested security header missing: strict-transport-security. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security
+ [600050] Apache/2.4.38 appears to be outdated (current is at least 2.4.66).
+ [999967] /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
+ [750500] /app/: Directory indexing found.
+ [001563] /app/: This might be interesting.
+ [999100] /public/: Uncommon header(s) 'x-debug-token' found, with contents: ecce77.
+ [750500] /src/: Directory indexing found.
+ [003584] /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
```

# rpcinfo_raw

```
┌──(root㉿kali)-[~]
└─# rpcinfo 192.168.57.8                  
   program version netid     address                service    owner
    100000    4    tcp6      ::.0.111               portmapper superuser
    100000    3    tcp6      ::.0.111               portmapper superuser
    100000    4    udp6      ::.0.111               portmapper superuser
    100000    3    udp6      ::.0.111               portmapper superuser
    100000    4    tcp       0.0.0.0.0.111          portmapper superuser
    100000    3    tcp       0.0.0.0.0.111          portmapper superuser
    100000    2    tcp       0.0.0.0.0.111          portmapper superuser
    100000    4    udp       0.0.0.0.0.111          portmapper superuser
    100000    3    udp       0.0.0.0.0.111          portmapper superuser
    100000    2    udp       0.0.0.0.0.111          portmapper superuser
    100000    4    local     /run/rpcbind.sock      portmapper superuser
    100000    3    local     /run/rpcbind.sock      portmapper superuser
    100005    1    udp       0.0.0.0.209.81         mountd     superuser
    100005    1    tcp       0.0.0.0.220.79         mountd     superuser
    100005    1    udp6      ::.168.104             mountd     superuser
    100005    1    tcp6      ::.212.117             mountd     superuser
    100005    2    udp       0.0.0.0.208.34         mountd     superuser
    100005    2    tcp       0.0.0.0.217.217        mountd     superuser
    100005    2    udp6      ::.130.192             mountd     superuser
    100005    2    tcp6      ::.182.87              mountd     superuser
    100005    3    udp       0.0.0.0.145.229        mountd     superuser
    100005    3    tcp       0.0.0.0.214.175        mountd     superuser
    100005    3    udp6      ::.169.241             mountd     superuser
    100005    3    tcp6      ::.183.163             mountd     superuser
    100003    3    tcp       0.0.0.0.8.1            nfs        superuser
    100003    4    tcp       0.0.0.0.8.1            nfs        superuser
    100227    3    tcp       0.0.0.0.8.1            nfs_acl    superuser
    100003    3    udp       0.0.0.0.8.1            nfs        superuser
    100227    3    udp       0.0.0.0.8.1            nfs_acl    superuser
    100003    3    tcp6      ::.8.1                 nfs        superuser
    100003    4    tcp6      ::.8.1                 nfs        superuser
    100227    3    tcp6      ::.8.1                 nfs_acl    superuser
    100003    3    udp6      ::.8.1                 nfs        superuser
    100227    3    udp6      ::.8.1                 nfs_acl    superuser
    100021    1    udp       0.0.0.0.191.252        nlockmgr   superuser
    100021    3    udp       0.0.0.0.191.252        nlockmgr   superuser
    100021    4    udp       0.0.0.0.191.252        nlockmgr   superuser
    100021    1    tcp       0.0.0.0.169.103        nlockmgr   superuser
    100021    3    tcp       0.0.0.0.169.103        nlockmgr   superuser
    100021    4    tcp       0.0.0.0.169.103        nlockmgr   superuser
    100021    1    udp6      ::.234.30              nlockmgr   superuser
    100021    3    udp6      ::.234.30              nlockmgr   superuser
    100021    4    udp6      ::.234.30              nlockmgr   superuser
    100021    1    tcp6      ::.180.175             nlockmgr   superuser
    100021    3    tcp6      ::.180.175             nlockmgr   superuser
    100021    4    tcp6      ::.180.175             nlockmgr   superuser
```