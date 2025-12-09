# 13 – Fail2ban: Basic Intrusion Rate-Limiting

Firewalls decide **who can talk to you**. Fail2ban decides **how many chances they get** before they are cut off.

On a hardened Linux system that exposes any service (SSH, web, etc.), fail2ban is a practical way to rate-limit brute-force and noisy probing.

## 1. Install fail2ban

On Debian/Ubuntu:

```bash
sudo apt-get update
sudo apt-get install fail2ban
````

Service should start automatically:

```bash
sudo systemctl status fail2ban
```

## 2. Do not edit jail.conf directly

Default config file:

```bash
/etc/fail2ban/jail.conf
```

Leave it alone. Instead create:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

`jail.local` overrides defaults in a minimal, trackable way.

## 3. Basic SSH jail (when SSH is exposed)

If you **intentionally** expose SSH (e.g. on a server, or internal admin machine), a minimal jail might look like:

```ini
[sshd]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 5
bantime  = 3600
findtime = 600
```

* `maxretry` – how many failures before ban
* `findtime` – window in seconds to observe failures
* `bantime` – ban duration in seconds

On a public-facing host with SSH, consider stricter settings and key-based auth only.

## 4. Integration with UFW

By default, fail2ban uses `iptables` actions. On systems where you standardized on UFW, you can use UFW-aware actions.

In `jail.local`:

```ini
[DEFAULT]
banaction = ufw
```

Or use one of the more specific UFW actions defined in `/etc/fail2ban/action.d/`, such as `ufw` or `ufw-reject`.

This makes fail2ban insert rules into UFW rather than managing iptables independently.

## 5. Example desktop/server split

* On a **workstation** with UFW set to:

  * deny incoming
  * allow outgoing
  * no exposed SSH

  fail2ban adds very little value; there is nothing to attack externally.

* On a **server** or admin box where SSH or HTTP(S) is intentionally exposed, fail2ban is useful:

  * Rate-limits brute-force attempts
  * Provides logs of abusive IPs
  * Can feed into other tooling (block lists, SIEM, etc.)

Do not deploy fail2ban just to tick a box. Deploy it where it actually has something to protect.

## 6. Monitoring

Check current jails:

```bash
sudo fail2ban-client status
```

Check a specific jail:

```bash
sudo fail2ban-client status sshd
```

Restart after changes:

```bash
sudo systemctl restart fail2ban
```

Fail2ban is not a silver bullet. It just makes abuse more expensive.
