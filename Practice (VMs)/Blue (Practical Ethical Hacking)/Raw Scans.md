# nmap_raw

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-14 20:40 -0400
Stats: 0:00:08 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 0.00% done
Stats: 0:00:23 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 37.50% done; ETC: 20:41 (0:00:35 remaining)
Nmap scan report for 192.168.57.5
Host is up (0.0026s latency).

PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
MAC Address: 08:00:27:12:B2:34 (Oracle VirtualBox virtual NIC)
Service Info: Host: WIN-845Q99OO4PP; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: WIN-845Q99OO4PP
|   NetBIOS computer name: WIN-845Q99OO4PP\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-08-14T20:41:51-04:00
| smb2-time: 
|   date: 2026-08-15T00:41:51
|_  start_date: 2026-08-15T00:28:54
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h20m00s, deviation: 2h18m33s, median: 0s
|_nbstat: NetBIOS name: WIN-845Q99OO4PP, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:12:b2:34 (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 66.36 seconds
```

# enum4linux_raw

```
┌──(kali㉿kali)-[~]
└─$ enum4linux -a 192.168.57.5               
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Aug 14 21:26:03 2026

 =========================================( Target Information )=========================================

Target ........... 192.168.57.5
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ============================( Enumerating Workgroup/Domain on 192.168.57.5 )============================


[+] Got domain/workgroup name: WORKGROUP


 ================================( Nbtstat Information for 192.168.57.5 )================================

Looking up status of 192.168.57.5
        WIN-845Q99OO4PP <00> -         B <ACTIVE>  Workstation Service
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WIN-845Q99OO4PP <20> -         B <ACTIVE>  File Server Service
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser

        MAC Address = 08-00-27-12-B2-34

 ===================================( Session Check on 192.168.57.5 )===================================
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+] Server 192.168.57.5 allows sessions using username '', password ''                                                                                                                                            
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 ================================( Getting domain SID for 192.168.57.5 )================================
                                                                                                                                                                                                                  
do_cmd: Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED                                                                                                                                            

[+] Can't determine if host is part of domain or part of a workgroup                                                                                                                                              
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 ===================================( OS information on 192.168.57.5 )===================================
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[E] Can't get OS info with smbclient                                                                                                                                                                              
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+] Got OS info for 192.168.57.5 from srvinfo:                                                                                                                                                                    
do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED                                                                                                                                            


 =======================================( Users on 192.168.57.5 )=======================================
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED                                                                                                                                              
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  

[E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED                                                                                                                                               
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 =================================( Share Enumeration on 192.168.57.5 )=================================
                                                                                                                                                                                                                  
do_connect: Connection to 192.168.57.5 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)                                                                                                                           

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
Unable to connect with SMB1 -- no workgroup available

[+] Attempting to map shares on 192.168.57.5                                                                                                                                                                      
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 ============================( Password Policy Information for 192.168.57.5 )============================
                                                                                                                                                                                                                  
Password:                                                                                                                                                                                                         

[E] Unexpected error from polenum:                                                                                                                                                                                
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  

[+] Attaching to 192.168.57.5 using a NULL share

[+] Trying protocol 139/SMB...

        [!] Protocol failed: Cannot request session (Called Name:192.168.57.5)

[+] Trying protocol 445/SMB...

        [!] Protocol failed: SMB SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.



[E] Failed to get password policy with rpcclient                                                                                                                                                                  
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  

 =======================================( Groups on 192.168.57.5 )=======================================
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+] Getting builtin groups:                                                                                                                                                                                       
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+]  Getting builtin group memberships:                                                                                                                                                                           
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+]  Getting local groups:                                                                                                                                                                                        
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+]  Getting local group memberships:                                                                                                                                                                             
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+]  Getting domain groups:                                                                                                                                                                                       
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[+]  Getting domain group memberships:                                                                                                                                                                            
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 ==================( Users on 192.168.57.5 via RID cycling (RIDS: 500-550,1000-1050) )==================
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
[E] Couldn't get SID: NT_STATUS_ACCESS_DENIED.  RID cycling not possible.                                                                                                                                         
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
 ===============================( Getting printer info for 192.168.57.5 )===============================
                                                                                                                                                                                                                  
do_cmd: Could not initialise spoolss. Error was NT_STATUS_ACCESS_DENIED                                                                                                                                           


enum4linux complete on Fri Aug 14 21:26:07 2026

```
