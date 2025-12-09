# 07 – System Auditing with Lynis

Lynis provides a high-signal baseline audit aligned with CIS and best practices.

## Installation

```bash
apt-get install lynis
````

## Run audit

```bash
sudo lynis audit system
```

## Interpretation

* Not every warning is actionable
* Fix blind spots, not cosmetic scores
* Re-run after major changes

Lynis does not secure your system. It shows you where you didn’t.
