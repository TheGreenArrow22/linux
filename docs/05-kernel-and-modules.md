# 05 – Kernel and Module Management

## Kernel version

```bash
uname -a
cat /proc/version
````

## Loaded modules

```bash
lsmod
```

## Inspect and manage modules

```bash
modinfo <module>
modprobe <module>
modprobe -r <module>
```

## Verify module activity

```bash
dmesg | grep <module>
```
