### Target Name: Dev
### Target IP: 192.168.57.8
### Attacker IP: 192.168.57.4
# -- Scanning & Enumeration --
**[x] Read [[S&E Methodology]]**
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
##### Proof:
![[Pasted image 20260818182519.png]]
# Operating System
#### ...
```
┌──(root㉿kali)-[~]
└─# nmap -T4 -sV -sC -O --script "vuln" -p 22,80,111,2049,8080,33155,36849,37197,51149 192.168.57.8
...

```

# -- Exploitation --
**[ ] Read [[Exploitation Methodology]]**
# -- Post-Exploitation --
**[ ] Read [[Post-Exploitation Methodology]]**
