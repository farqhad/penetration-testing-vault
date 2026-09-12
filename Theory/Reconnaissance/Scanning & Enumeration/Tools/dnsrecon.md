### -d (Domain)

***explanation:*** **specifies the target domain to enumerate; this is the base flag almost every dnsrecon scan is built around**

***usage:*** **dnsrecon -d example.com**

### -t (Type of enumeration)

***explanation:*** **sets which enumeration technique to run (std, brt, axfr, srv, etc.); controls what kind of records/attack dnsrecon attempts**

***usage:*** **dnsrecon -d example.com -t std**

### -t std (Standard)

***explanation:*** **runs standard record enumeration (A, AAAA, MX, NS, SOA, TXT); the default first-pass recon for a domain**

***usage:*** **dnsrecon -d example.com -t std**

### -t axfr (Zone Transfer)

***explanation:*** **attempts a DNS zone transfer against the domain's nameservers; if a misconfigured server allows it, you get the entire DNS zone in one shot (a jackpot finding)**

***usage:*** **dnsrecon -d example.com -t axfr**

### -t brt (Bruteforce)

***explanation:*** **bruteforces subdomains using a wordlist; finds hosts that aren't publicly listed and won't appear in a standard scan**

***usage:*** **dnsrecon -d example.com -t brt -D /usr/share/wordlists/subdomains.txt**

### -D (Dictionary)

***explanation:*** **specifies the wordlist file used for subdomain bruteforcing; required when running the brt enumeration type**

***usage:*** **dnsrecon -d example.com -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt**

### -t srv (SRV Records)

***explanation:*** **enumerates SRV records to reveal services like LDAP, Kerberos, SIP, and XMPP; especially useful for fingerprinting Active Directory environments**

***usage:*** **dnsrecon -d example.com -t srv**

### -n (Nameserver)

***explanation:*** **specifies which nameserver to query instead of the system default; useful for aiming a zone transfer or query at a specific target DNS server**

***usage:*** **dnsrecon -d example.com -t axfr -n ns1.example.com**

### -r (Reverse Lookup Range)

***explanation:*** **performs reverse DNS (PTR) lookups across an IP range, resolving IPs back to hostnames; great for mapping internal hosts and discovering naming schemes without a forward domain**

***usage:*** **dnsrecon -r 192.168.1.0/24 -n 192.168.1.10**

### -a (AXFR + Standard)

***explanation:*** **runs a standard enumeration and attempts a zone transfer against every nameserver found; convenient one-flag combo for first contact**

***usage:*** **dnsrecon -d example.com -a**

# Common Usage Combos

### Standard First-Pass Recon

**dnsrecon -d example.com -t std**

### Reverse Lookup Across a Range (Internal Host Mapping)

**dnsrecon -r 10.10.10.0/24 -n 10.10.10.1**

### Zone Transfer Attempt (Quick Win Check)

**dnsrecon -d example.com -t axfr**

### Subdomain Bruteforce

**dnsrecon -d example.com -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt**

### Active Directory Service Discovery

**dnsrecon -d example.com -t srv**

### Targeted Zone Transfer Against Specific Nameserver

**dnsrecon -d example.com -t axfr -n ns1.example.com**