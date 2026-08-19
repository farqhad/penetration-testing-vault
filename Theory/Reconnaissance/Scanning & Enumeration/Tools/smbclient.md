### -L (List)

***explanation:*** **displays a list of all available network shares on the target server**

***usage:*** **smbclient -L //192.168.1.10**

### -N (No Pass)

***explanation:*** **suppresses the password prompt and forces a null session/anonymous login attempt**

***usage:*** **smbclient -L //192.168.1.10 -N**

### -U (Username)

***explanation:*** **specifies a known username to authenticate with instead of attempting an anonymous connection**

***usage:*** **smbclient //192.168.1.10/sharename -U "admin"**

# Common Usage Combos

### Anonymous Share Listing

**smbclient -L //192.168.1.10 -N**

### Connect to a Share Anonymously

**smbclient //192.168.1.10/sharename -N**

### Connect with Known Credentials

**smbclient //192.168.1.10/sharename -U "admin"**

### Connect with All Known Credentials (Inline Password) 

**smbclient //192.168.1.10/sharename -U "admin%password123"**
