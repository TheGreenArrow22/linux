# 08 – CIS Filesystem and Partition Controls

Separate partitions reduce blast radius.

Critical paths:

- /tmp
- /var
- /opt
- /home
- /media

Inspect:

```bash
cat /etc/fstab
````

Ensure mount options include:

* nodev
* nosuid
* noexec (where applicable)

Improper mounts are one of the oldest privilege escalation paths on Linux.
