# 09 – CIS Services and Boot Hardening

## Disable legacy inetd services

Check:

```bash
cat /etc/inetd.conf
````

Confirm Telnet, FTP, rsh, rlogin, tftp, imap, pop3 are disabled.

## Disable unused services

Examples:

* Samba
* NFS
* NIS
* Apache
* MySQL
* PostgreSQL
* Webmin
* Squid

Inspect:

```bash
systemctl list-unit-files --state=enabled
```

Unused services increase attack surface. Period.
