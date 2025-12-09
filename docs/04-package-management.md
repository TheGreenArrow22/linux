# 04 – Package Management (APT)

## Installing software

```bash
apt-get install packagename
````

## Removing software

```bash
apt-get remove packagename
apt-get purge packagename
```

## Cleanup unused dependencies

```bash
apt autoremove
```

## Updates

```bash
apt-get update
apt-get upgrade
```

Patch latency is one of the most common root causes of compromise.
