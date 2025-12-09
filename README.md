# Linux Privacy & Hardening Toolkit

A practical, command-driven hardening baseline for Debian and Ubuntu systems.

This repository is designed for defenders, researchers, and privacy-conscious users who want **visibility, control, and verification**, not blind automation.

Nothing here should be applied without understanding what it does.

## Scope

This toolkit covers:

- Core Linux command-line literacy
- Process inspection and filtering
- File and directory manipulation
- Package management and updates
- Kernel introspection and module control
- Firewalling with UFW
- System auditing with Lynis
- CIS-aligned filesystem, service, SSH, and kernel hardening
- Explicit tradeoffs and rationale for each control

Primary target:

- Debian
- Ubuntu and Ubuntu-derived systems

Secondary compatibility:

- Other systemd-based distributions (with adaptation)

## Philosophy

- No one-size-fits-all scripts
- Explicit trust boundaries
- Assume compromise is possible
- Prefer detection and verification over assumptions

## Quick start

```bash
git clone https://github.com/TheGreenArrow22/linux.git
cd linux
````

Start here:

```bash
open docs/00-overview.md
```

Apply sections selectively according to your threat model.

## Disclaimer

Some controls in this repository:

* Disable services
* Restrict networking
* Modify kernel behavior

Test everything in a VM before touching production or personal machines.
