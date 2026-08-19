# OSINT Checklists

> External recon on the target organisation — work through each area top to bottom.

### 1) General Information & Social Media Recon

- [ ] Utilize Google Fu for advanced target search queries.
- [ ] Map company structure and employee names using LinkedIn and Twitter.
- [ ] Consult the External Tool List for any niche investigative needs.

### 2) Email Enumeration & Verification

- [ ] Discover target employee emails using hunter.io and ClearBit.
- [ ] Verify the validity of the discovered email addresses using emailhippo.

### 3) Credential & Breach Hunting

- [ ] Cross-reference discovered emails and usernames against known database leaks using DeHashed.
- [ ] Identify and crack found password hashes using Hashes.org.

### 4) Subdomain Enumeration

- [ ] Search for registered SSL certificates using crt.sh.
- [ ] Run fast, passive subdomain scraping using Subfinder.
- [ ] Execute a comprehensive, deep subdomain scan using OWASP Amass.
- [ ] Fuzz for undocumented subdomains using FFUF.
- [ ] Verify which of the discovered subdomains are actually alive using tomnomnom/httprobe.

### 5) Technology Fingerprinting

- [ ] Profile the target's web technology stack using builtwith.com.
- [ ] Identify frontend frameworks and server versions actively using the Wappalyzer browser extension.
- [ ] Run command-line fingerprinting using whatweb.
- [ ] Intercept traffic and analyze raw HTTP headers for technology disclosures using Burpsuite.
