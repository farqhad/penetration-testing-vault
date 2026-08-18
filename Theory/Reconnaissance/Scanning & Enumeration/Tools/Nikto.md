### -h (host)
***explanation:*** **specifies the target IP address, hostname, or URL to be scanned.**
***usage(1):*** **nikto -h 192.168.0.1**
***usage(2):*** **nikto -h 192.168.57.3:80**

### -p (port)
***explanation:*** **specifies the port number to scan if the web server is not running on the default port 80.**
***usage(1):*** **nikto -h 192.168.0.1 -p 8080**
***usage(2):*** **nikto -h 192.168.0.1 -p 80,443,8080**

### -o (output)
***explanation:*** **saves the scan results to a specified file name.**
***usage:*** **nikto -h 192.168.0.1 -o scan_results.txt**

### -Format (Format)
***explanation:*** **specifies the output file format (e.g., csv, htm, txt, xml). Usually paired with the -o flag.**
***usage:*** **nikto -h 192.168.0.1 -o report.htm -Format htm**

### -Tuning (Tuning)
***explanation:*** **controls which specific types of tests are executed, allowing for faster, targeted scans (e.g., passing '4' runs only injection tests).**
***usage(1):*** **nikto -h 192.168.0.1 -Tuning 4**
***usage(2):*** **nikto -h 192.168.0.1 -Tuning x**

### -nossl (No SSL)
***explanation:*** **disables the use of SSL, forcing the tool to scan using plain HTTP even if it detects a traditional SSL port.**
***usage:*** **nikto -h 192.168.0.1 -p 443 -nossl**

### -ssl (Force SSL)
***explanation:*** **forces the tool to use SSL for the connection, useful if the server is running HTTPS on a non-standard port.**
***usage:*** **nikto -h 192.168.0.1 -p 8080 -ssl**