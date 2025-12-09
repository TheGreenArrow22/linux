# 12 – Mandatory Access Control: AppArmor and SELinux

Discretionary Unix permissions (user/group/other + rwx) are not enough. Mandatory Access Control (MAC) adds **policy that even root is constrained by**.

On Debian and Ubuntu, the default MAC framework is **AppArmor**. SELinux is usually seen on RHEL/CentOS/Fedora, but can be used on Debian-based systems with work.

Do not attempt to run AppArmor and SELinux at the same time.

---

## 1. AppArmor on Debian/Ubuntu

### 1.1 Check AppArmor status

```bash
sudo aa-status
````

You should see:

* Whether AppArmor is enabled
* Which profiles are loaded
* Which profiles are in `enforce` vs `complain` mode

If `aa-status` is missing:

```bash
sudo apt-get install apparmor apparmor-utils
```

### 1.2 Ensure AppArmor is enabled at boot

Check the kernel command line:

```bash
cat /proc/cmdline
```

You should **not** see `apparmor=0`. If you see `apparmor=0` or `security=...` pointing to something else, AppArmor may be disabled.

On systemd-based Debian/Ubuntu, AppArmor is usually enabled by default if the kernel supports it.

### 1.3 Profiles

AppArmor uses text profiles, typically under:

```bash
/etc/apparmor.d/
```

Examples:

* `/etc/apparmor.d/usr.bin.man`
* `/etc/apparmor.d/usr.sbin.sshd`
* `/etc/apparmor.d/usr.bin.evince`

### 1.4 Enforcing and complain modes

Put a profile into enforce mode:

```bash
sudo aa-enforce /etc/apparmor.d/usr.sbin.sshd
```

Put it into complain mode:

```bash
sudo aa-complain /etc/apparmor.d/usr.sbin.sshd
```

* **Enforce**: violations are blocked and logged.
* **Complain**: violations are logged but not blocked.

Use complain mode when testing new profiles to avoid breaking services blindly.

### 1.5 Restarting services and reloading profiles

After changing a profile, reload it:

```bash
sudo apparmor_parser -r /etc/apparmor.d/<profile-name>
```

Then restart the associated service, for example:

```bash
sudo systemctl restart ssh
```

### 1.6 Operational strategy

* Leave vendor-supplied profiles in enforce mode unless they clearly break legitimate use.
* Put high-risk network-facing services under AppArmor with tight profiles.
* For your own tools, start with complain mode, watch logs, then move to enforce once the noise is understood.

---

## 2. SELinux (overview, not default on Debian/Ubuntu)

SELinux is a more complex MAC framework, heavily used in RHEL/CentOS/Fedora ecosystems.

On those systems, check status:

```bash
getenforce
sestatus
```

Typical modes:

* **enforcing** – policy enforced
* **permissive** – violations logged but not blocked
* **disabled** – no SELinux

Configuration is usually in:

```bash
/etc/selinux/config
```

Example:

```text
SELINUX=enforcing
SELINUXTYPE=targeted
```

### 2.1 On Debian/Ubuntu

SELinux can be installed, but it is not the default. Doing so without understanding the implications will break things.

Unless you have a concrete reason and know exactly why SELinux is needed on Debian/Ubuntu, stick to:

* AppArmor in enforce mode
* Tight service exposure via UFW
* Good filesystem and SSH hardening

---

## 3. Logging and diagnostics

For AppArmor, check:

```bash
sudo journalctl -k | grep -i apparmor
```

or:

```bash
sudo dmesg | grep -i apparmor
```

For SELinux (on systems that use it):

```bash
sudo ausearch -m avc -ts recent
```

MAC is only useful if you actually read the logs and refine policy. Otherwise it becomes “that thing that randomly blocks stuff” until someone disables it.
