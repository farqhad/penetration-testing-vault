### Target Name: Blue

### Target IP: 192.168.57.5

### Attacker IP: 192.168.57.4

### Started: ~2:30 AM

### Rooted: ~4:20 AM

# Operating System

### Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)

**No longer supported!!! Bad.**

# Open Ports

### 135/TCP (RPC/MSRPC)

**Microsoft Windows RPC**

### 139/TCP (SMB/NETBIOS-SSN)

**Microsoft Windows netbios-ssn**

### 445/TCP (SMB/MICROSOFT-DS)

**Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)**

### Additional/Non-Standard (all RPC/MSRPC)

**49152/tcp Microsoft Windows RPC**

**49153/tcp Microsoft Windows RPC**

**49154/tcp Microsoft Windows RPC**

**49155/tcp Microsoft Windows RPC**

**49157/tcp Microsoft Windows RPC**

# SMB (445/139)

### Version

**via nmap: SMB 2.1**

![Pasted image 20260815032005](../../assets/Pasted%20image%2020260815032005.png)

**via metasploit (auxiliary/scanner/smb/smb_version): SMB 2.1**

![](file:///C:/Users/Farhad/Pictures/Screenshots/Screenshot%202026-08-15%20025752.png)

**via Wireshark: SMB2 (as in 2.x)**

![Pasted image 20260815031000](../../assets/Pasted%20image%2020260815031000.png)

### Connection

**- Anonymous credentials access via smbclient possible to //192.168.57.5/IPC$**

**- NOT possible to //192.168.57.5/C$ and //192.168.57.5/ADMIN$**

### Credentials

**Known Usernames (via enum4linux): administrator, guest, krbtgt, domain admins, root, bin, none**

![Pasted image 20260815033654](../../assets/Pasted%20image%2020260815033654.png)

**Workgroup Name (via enum4linux): WORKGROUP**

![Pasted image 20260815033736](../../assets/Pasted%20image%2020260815033736.png)

**Allows anonymous sessions (via enum4linux):**

![Pasted image 20260815033859](../../assets/Pasted%20image%2020260815033859.png)

**Doesn't have workgroups on SMB1 (via enum4linux):**

![Pasted image 20260815034031](../../assets/Pasted%20image%2020260815034031.png)

**Doesn't allow sessions over port 139, on which most likely the outdated SMB1 runs (via enum4linux):**

![Pasted image 20260815034233](../../assets/Pasted%20image%2020260815034233.png)

**Doesn't even have a resource on port 139 (via smbclient):**

![Pasted image 20260815034423](../../assets/Pasted%20image%2020260815034423.png)

### Vulnerability Research (Manual)

**Potentially vulnerable to EternalBlue Remote Code Execution (MS17-010)**

![Pasted image 20260815041823](../../assets/Pasted%20image%2020260815041823.png)

![Pasted image 20260815041853](../../assets/Pasted%20image%2020260815041853.png)

### Vulnerability Scanning (Nessus)

**Potentially vulnerable to MS11-030: Remote Code Execution and/or DoS via DNS Resolution Vulnerability**

**Potentially vulnerable to MS16-047: Vulnerability in SAM and LSAD Remote Protocols that allows for privilege escalation.**

**Potentially vulnerable to ETERNALBLUE, ETERNALCHAMPION, ETERNALROMANCE, and ETERNALSYNERGY (MS17-010): Remote Code Execution**

![Pasted image 20260815041738](../../assets/Pasted%20image%2020260815041738.png)

# RPC

### Version

**Not apparent in any of the scans and/or Wireshark**

### Connection

**- Anonymous access denied**

![Pasted image 20260815032849](../../assets/Pasted%20image%2020260815032849.png)

# Exploitation

**Vulnerable to EternalBlue Remote Code Execution (MS17-010)**

**The exploit failed on initial execution, but succeeded on the second attempt without payload modification.**

![Pasted image 20260815043220](../../assets/Pasted%20image%2020260815043220.png)

![Screenshot 2026-08-15 043100](../../assets/Screenshot%202026-08-15%20043100.png)

***forgot ipconfig in the end as proof of machine hacked***
