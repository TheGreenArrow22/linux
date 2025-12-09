# 06 – Firewall and Networking (UFW)

For a typical Debian/Ubuntu workstation that primarily:

- Browses the web
- Uses email
- Connects out to APIs and cloud services

you usually do *not* need any inbound ports open.

The baseline is:

- Deny **incoming** by default
- Allow **outgoing** by default
- Only open inbound ports when you deliberately run a network-facing service and know why

## 1. Install UFW

On Debian/Ubuntu:

```bash
sudo apt-get update
sudo apt-get install ufw
````

## 2. Set sensible defaults (desktop profile)

Deny all incoming connections:

```bash
sudo ufw default deny incoming
```

Allow all outgoing traffic:

```bash
sudo ufw default allow outgoing
```

This means:

* Nothing on the internet can initiate a connection into your machine by default.
* Your machine can still browse, update, and reach external services.

## 3. Enable UFW

```bash
sudo ufw enable
```

UFW will warn you that enabling it may disrupt existing connections. On a typical desktop with no self-hosted services, this is acceptable.

Check status:

```bash
sudo ufw status verbose
```

You should see:

* `Status: active`
* `Default: deny (incoming), allow (outgoing)`

## 4. Adding rules when needed

If at some point you **intentionally** expose a service (for example an internal-only web app on port 8080), you would explicitly allow it:

```bash
sudo ufw allow 8080/tcp
```

Or more specific:

```bash
sudo ufw allow from 192.168.1.0/24 to any port 8080 proto tcp
```

This keeps your default stance closed, and you only punch holes you can justify.

## 5. Reviewing and cleaning rules

List rules with numbers:

```bash
sudo ufw status numbered
```

Delete a rule by number:

```bash
sudo ufw delete <rule-number>
```

Example:

```bash
sudo ufw delete 3
```

The firewall should reflect your actual usage, not a random collection of rules from old experiments.
