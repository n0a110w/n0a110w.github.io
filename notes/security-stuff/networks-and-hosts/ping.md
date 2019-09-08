---
layout: default
title: ping
parent: Network and Host Discovery
grand_parent: security stuff
nav_order: 2
---

## Scanning with ping
### ping in bash:
```bash
for i in {1..254}; do ping -c 1 -W 1 172.31.6.$i | grep 'from'; done
```
### ping in windows
```bash
for /L %V in (1 1 254) do PING -n 1 192.168.2.%V | FIND /I "Reply"
```

---

