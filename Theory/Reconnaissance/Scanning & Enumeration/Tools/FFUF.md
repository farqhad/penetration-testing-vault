### -u (URL)
***explanation:*** **specifies the target URL and defines where the "FUZZ" keyword will be injected.**
***usage(1):*** **ffuf -u httр://192.168.0.1/FUZZ -w wordlist.txt**
***usage(2):*** **ffuf -u httр://192.168.0.1/api/FUZZ -w parameters.txt**

### -w (Wordlist)
***explanation:*** **specifies the path to the wordlist file containing the payloads to replace the "FUZZ" keyword.**
***usage(1):*** **ffuf -w /usr/share/wordlists/dirb/common.txt -u httр://10.10.10.10/FUZZ**
***usage(2):*** **ffuf -w users.txt:W1 -w passwords.txt:W2 -u httр://10.10.10.10/login?u=W1&p=W2**

### -c (Colorize)
***explanation:*** **colorizes the output based on the HTTP status codes, making it much easier to read in the terminal.**
***usage:*** **ffuf -c -w common.txt -u httр://192.168.0.1/FUZZ**

### -mc (Match Code)
***explanation:*** **tells FFUF to only show results that return the specified HTTP status codes.**
***usage(1):*** **ffuf -mc 200 -w common.txt -u httр://10.10.10.10/FUZZ**
***usage(2):*** **ffuf -mc 200,301,302 -w common.txt -u httр://10.10.10.10/FUZZ**

### -fc (Filter Status Code)
***explanation:*** **tells FFUF to hide or ignore specific HTTP status codes (e.g., hiding a bunch of 403 Forbidden errors).**
***usage(1):*** **ffuf -fc 403 -w common.txt -u httр://10.10.10.10/FUZZ**
***usage(2):*** **ffuf -fc 403,404 -w common.txt -u httр://10.10.10.10/FUZZ**

### -fs (Filter Size)
***explanation:*** **filters out HTTP responses that have a specific file size, useful for ignoring generic default pages that all return a 200 OK but are identical in size.**
***usage:*** **ffuf -fs 4242 -w common.txt -u httр://10.10.10.10/FUZZ**

### -H (Header)
***explanation:*** **adds custom headers to the HTTP request, often used for fuzzing virtual host names or passing authentication tokens.**
***usage(1):*** **ffuf -H "Host: FUZZ.target.com" -w subdomains.txt -u httр://target.com**
***usage(2):*** **ffuf -H "Authorization: Bearer token123" -w api.txt -u httр://10.10.10.10/FUZZ**
### -X (HTTP Method)
***explanation:*** **specifies the HTTP method to use for the request (e.g., POST, PUT, DELETE). FFUF defaults to a standard GET request if this flag is not used.**
***usage(1):*** **ffuf -c -ic -X POST -d "user=admin&pass=FUZZ" -w passwords.txt -u httр://10.10.10.10/login**
***usage(2):*** **ffuf -c -ic -X FUZZ -w methods.txt -u httр://10.10.10.10/api/users -fc 405,403**
### -d (Data)
***explanation:*** **specifies the raw data to be sent in the body of the HTTP request. This is strictly required when fuzzing POST or PUT endpoints so the server has data to process.**
***usage(1):*** **ffuf -c -ic -X POST -d "username=admin&password=FUZZ" -w rockyou.txt -u httр://10.10.10.10/login -H "Content-Type: application/x-www-form-urlencoded"**
***usage(2):*** **ffuf -c -ic -X POST -d '{"email":"test@target.com","role":"FUZZ"}' -w parameters.txt -u httр://10.10.10.10/api/users -H "Content-Type: application/json"**
### -e (Extension)
***explanation:*** **automatically appends specific file extensions to the end of every word in the wordlist.**
***usage:*** **ffuf -e .php,.txt -w common.txt -u httр://10.10.10.10/FUZZ**
### -ic (Ignore Comments)
***explanation:*** **instructs FFUF to ignore comments in wordlists, preventing the tool from sending the wordlist's metadata or copyright headers as payloads to the target.**
***usage:*** **ffuf -ic -w /usr/share/wordlists/dirb/common.txt -u httр://10.10.10.10/FUZZ**
### -p (Pause/Delay)
***explanation:*** **specifies the delay in seconds between each request. Primarily used to evade Web Application Firewalls (WAFs), bypass rate-limiting restrictions (like 429 Too Many Requests errors), or prevent crashing fragile/embedded targets.**
***usage(1):*** **ffuf -p 0.1 -c -ic -w /usr/share/wordlists/dirb/common.txt -u httр://10.10.10.10/FUZZ -fc 404**
***usage(2):*** **ffuf -t 1 -p 1.5 -c -ic -w subdomains.txt -H "Host: FUZZ.target.com" -u httр://target.com -fs 4242**
# Common Usage Combos
## (CHANGE 'p' TO LATIN LAYOUT!!!)
### Backend Technology Profiling (Standard Apache/Nginx)
**ffuf -c -ic -w extensions.txt -u httр://10.10.10.10/indexFUZZ -fc 404**
### Backend Technology Profiling (IIS / Microsoft Specific)
**ffuf -c -ic -w extensions.txt -u httр://10.10.10.10/defaultFUZZ -fc 404**
### Combined Directory & File Fuzzing
**ffuf -c -ic -t 2/40(def)/100/200 -recursion -w /usr/share/wordlists/dirb/common.txt -e .php -u httр://192.168.57.134:80/FUZZ -fc 404**
**ffuf -c -ic -t 2/40(def)/100/200 -recursion -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -e .php -u httр://192.168.57.134:80/FUZZ -fc 404**
### Standard Directory Sweep (Clean & Colored)
**ffuf -c -ic -w /usr/share/wordlists/dirb/common.txt -u httр://10.10.10.10/FUZZ -fc 404**
### Deep File Hunting (Extension Brute-Forcing)
**ffuf -c -ic -w /usr/share/wordlists/dirb/common.txt -e .php,.txt,.bak -u httр://10.10.10.10/FUZZ -fc 404**
### Subdomain & VHost Discovery (Filtering Default Sizes)
**ffuf -c -ic -w subdomains.txt -H "Host: FUZZ.target.com" -u httр://target.com -fs 4242**
### Authenticated Parameter Fuzzing (Strict Matching)
**ffuf -c -ic -w parameters.txt -H "Authorization: Bearer token123" -u httр://10.10.10.10/api.php?FUZZ=test -mc 200**

# Extension Lists (based on technology used)
### FFUF Extensions: PHP / Standard LAMP Stack
**.php,.txt,.bak,.old,.log,.zip,.sql**
### FFUF Extensions: Microsoft IIS / ASP.NET
**.aspx,.config,.txt,.bak,.old,.log,.zip**
### FFUF Extensions: Java / Apache Tomcat
**.jsp,.war,.txt,.bak,.old,.log,.zip**
### FFUF Extensions: Static / Unknown 
**.html,.js,.txt,.bak,.old,.zip**
