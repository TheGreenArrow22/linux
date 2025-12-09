# 10 – SSH and Authentication Hardening

Inspect:

```bash
cat /etc/ssh/sshd_config
````

Confirm:

* Protocol 2 only
* IgnoreRhosts yes
* PermitEmptyPasswords no
* HostbasedAuthentication no
* PermitRootLogin no
* LoginGracetime 10

Reload SSH safely after edits:

```bash
sshd -t
systemctl reload ssh
```

SSH misconfigurations remain one of the top initial access vectors.
