
> External recon on the target organisation — work through each area top to bottom. Findings feed forward: names → usernames → emails → breaches; domains → live hosts → tech.

### 1) General Information & Social Media Recon

- [ ] Utilize Google Fu for advanced target search queries.
- [ ] Map company structure and employee names using LinkedIn and Twitter.
- [ ] Take any usernames / handles you find and enumerate their accounts across 840+ sites with Aliens_eye (AI-assisted — correlates likely matches and follows linked usernames).
- [ ] Geolocate any photos you surface (offices, badges, visible backgrounds) with Netryx-Astra-V2 to pin physical locations.
- [ ] Consult the External Tool List for any niche investigative needs.

### 2) Email Enumeration & Verification

- [ ] Discover target employee emails using hunter.io and ClearBit.
- [ ] Pull any additional addresses via MailAccess.
- [ ] Verify the validity of the discovered email addresses using emailhippo.

### 3) Credential & Breach Hunting

> Feed the emails, usernames, and names gathered above into each source.

- [ ] Cross-reference discovered emails and usernames against known database leaks using DeHashed (best coverage — start here).
- [ ] Check pentester.com for additional breach / credential exposure.
- [ ] Identify and crack any found password hashes using Hashes.org.

### 4) Subdomain & Infrastructure Enumeration

- [ ] Search for registered SSL certificates using crt.sh.
- [ ] Run fast, passive subdomain scraping using Subfinder.
- [ ] Execute a comprehensive, deep subdomain scan using OWASP Amass (thorough but slow).
- [ ] Fuzz for undocumented subdomains using FFUF.
- [ ] Map exposed hosts, open ports, services, and banners with Shodan; cross-reference certificates and services with Censys.
- [ ] Verify which of the discovered subdomains/hosts are actually alive using tomnomnom/httprobe — this filtered list feeds Technology Fingerprinting below.

### 5) Technology Fingerprinting

> Run these against the live hosts confirmed in the previous step.

- [ ] Profile the target's web technology stack using builtwith.com.
- [ ] Identify frontend frameworks and server versions actively using the Wappalyzer browser extension.
- [ ] Run command-line fingerprinting using whatweb.
- [ ] Intercept traffic and analyze raw HTTP headers for technology disclosures using Burpsuite.

### 6) Dark Web

- [ ] Run VoidAccess against the org name / domain to auto-sweep Tor, paste sites, and clearnet for leaked credentials, mentions, and IOCs.
- [ ] Crawl any specific `.onion` sites or leads with TorBot.
