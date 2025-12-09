# 11 – CIS Kernel Network Tuning

Disable dangerous behaviors.

## Source routing

```bash
cat /proc/sys/net/ipv4/conf/all/accept_source_route
````

Should be `0`.

## ICMP broadcast responses

```bash
cat /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts
```

Should be `1`.

## IP forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward
cat /proc/sys/net/ipv6/ip_forward
```

Should be `0`.

IP forwarding on endpoints enables man-in-the-middle behavior. Disable unless routing is intentional.

