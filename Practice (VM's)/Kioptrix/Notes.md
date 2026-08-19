### Target Name: Kioptrix Level 1

### Target IP: 192.168.57.3

### Attacker IP: 192.168.57.4

# Operating System

### Red-Hat/Linux

# Open Ports *([](Raw%20Scans.md#nmap_raw|view%20raw%20scan))*

### 22/TCP (SSH)

**runs on: OpenSSH 2.9p2 (protocol 1.99)**

### 80/TCP (HTTP)

**runs on: Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)**

### 443/TCP (HTTPS)

**runs on: Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b**

### 111/TCP (RPCBIND)

### 139/TCP (SMB/NETBIOS-SSN)

**runs on: Samba smbd (workgroup: MYGROUP)**

### 32768/TCP (RPCBIND, status service)

# 80/TCP (HTTP)

### Default Apache web page on Port 80 - PHP

![Pasted image 20260813114144](../../assets/Pasted%20image%2020260813114144.png)

### Information disclosure - 404 Page

![Pasted image 20260813114452](../../assets/Pasted%20image%2020260813114452.png)

### Server headers disclose version information

![Pasted image 20260813114933](../../assets/Pasted%20image%2020260813114933.png)

### Webalizer 2.01 - httр://192.168.57.3/usage

![Pasted image 20260813114539](../../assets/Pasted%20image%2020260813114539.png)

# 139/TCP (SMB/NETBIOS-SSN)

### Anonymous access allowed for IPC$

### No anonymous access for ADMIN$

### No listing allowed for IPC$

### Samba 2.2.1a - displayed in WireShark

![Pasted image 20260813115427](../../assets/Pasted%20image%2020260813115427.png)

# Vulnerability Scans *([](Raw%20Scans.md#nikto_raw|view%20raw%20scan))*

### Nikto - Port 80

**1) mod_ssl/2.8.4 is vulnerable to a remote buffer overflow (may allow a remote shell)**

**2) Apache/1.3.20 is vulnerable to remote DoS, code execution, and local buffer overflows**

**3) /usage/ directory found (Webalizer may be installed)**

**4) /test.php file found (requires manual investigation)**

**5) /manual/ and /icons/ directories have directory indexing enabled**

**6) HTTP TRACE method is active (vulnerable to Cross-Site Tracing)**

# Vulnerability Research (Manual)

### 80/443 - Potentially vulnerable to OpenFuck/OpenLuck

### 139 - Potentially vulnerable to Trans2Open

### 22 - Potentially vulnerable to Kerberos 4 TGT/AFS Token Buffer Overflow

### Linux Kernel 2.4.7-10 (RedHat) - Potentially vulnerable to Local Privilege Escalation (ptrace/kmod)

# Successful Exploitation

### Non-root gained with OpenFuck + escalated to root with ptrace/kmod

![Pasted image 20260815003348](../../assets/Pasted%20image%2020260815003348.png)

![Pasted image 20260815004130](../../assets/Pasted%20image%2020260815004130.png)

### Root gained with SSH Hydra Brute-Force attack. Weak password for root: 1234. + undetected malicious activity (brute-force)

![Pasted image 20260815004851](../../assets/Pasted%20image%2020260815004851.png)

![Pasted image 20260815004947](../../assets/Pasted%20image%2020260815004947.png)

### `ifconfig` available with SSH, opening an opportunity for pivoting

![Pasted image 20260815005042](../../assets/Pasted%20image%2020260815005042.png)