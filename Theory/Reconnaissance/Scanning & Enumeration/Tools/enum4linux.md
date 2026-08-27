### -a (All)

***explanation:*** **runs all standard enumeration checks; this is the loudest, heaviest scan, but usually the only one you need**

***usage:*** **enum4linux -a 192.168.1.10**

### -U (Users)

***explanation:*** **attempts to extract the full list of user accounts from the server via RPC**

***usage:*** **enum4linux -U 192.168.1.10**

### -S (Shares)

***explanation:*** **enumerates all available network shares on the target**

***usage:*** **enum4linux -S 192.168.1.10**

### -P (Password Policy)

***explanation:*** **pulls the domain or local password policy; extremely useful for knowing if an account will lock out before you start brute-forcing**

***usage:*** **enum4linux -P 192.168.1.10**

### -u (username) & -p (password)

***explanation:*** **specifies a known username and password to authenticate with instead of attempting an anonymous null session**

***usage:*** **enum4linux -u "admin" -p "password123" -a 192.168.1.10**

### -o (OS Information)

***explanation:*** **extracts operating system and Samba server version details without running the entire heavy enumeration suite**

***usage:*** **enum4linux -o 192.168.1.10**

### enum4linux-ng (Next-Gen)

***explanation:*** **a modernized Python rewrite of enum4linux that outputs structured, color-coded tables and filters out unnecessary RPC clutter**

***usage(1):*** **enum4linux-ng 192.168.1.10**

***usage(2):*** **enum4linux-ng -O 192.168.1.10**

# Common Usage Combos

### Targeted OS Version Check

**enum4linux -o 192.168.1.10**

### Modern Structured Enumeration Scan

**enum4linux-ng 192.168.1.10**

### The All-In-One (Anonymous/Null Session)

**enum4linux -a 192.168.1.10**

### Authenticated All-In-One (Known Credentials)

**enum4linux -u "admin" -p "password123" -a 192.168.1.10**

### Targeted Enumeration (Lean & Focused)

**enum4linux -U -S -P 192.168.1.10**
