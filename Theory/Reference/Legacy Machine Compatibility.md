# Legacy Machine Compatibility

> Shared reference for old boxes (Windows XP/2003, ancient Linux / SSL). Linked from both the S&E and Exploitation methodologies — edit it here, once.

## How to spot a legacy machine (typical errors)

- `kex error` **(key exchange)**
- `no match for method`
- `protocol negotiation failed`
- `SMB1 disabled` **or** `NT_STATUS_CONNECTION_RESET`
- `SSL_ERROR_UNSUPPORTED_VERSION`

## Fixing compatibility

**First try:** `kali-tweaks → Hardening → allow Wide Compatibility`

**Didn't help? Force it manually:**

- **SSH / Hydra** (server rejects modern ciphers):
  `-oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc`
  *(e.g. `ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -c 3des-cbc user@IP`)*
- **SMBv1** (modern smbclient disables NT1 by default; Windows XP/2003 throwing `NT_STATUS_CONNECTION_RESET`):
  `smbclient -L //IP/ --option='client min protocol=NT1'`
- **Legacy SSL web server** (`SSL_ERROR_UNSUPPORTED_VERSION` — force the TLS downgrade + lower the security level):
  `curl -k --tlsv1.0 --ciphers DEFAULT@SECLEVEL=0 https://IP`
