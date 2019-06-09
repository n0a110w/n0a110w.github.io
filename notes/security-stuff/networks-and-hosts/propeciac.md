---
layout: default
title: propecia.c 
parent: Network and Host Discovery
grand_parent: security stuff
nav_order: 2
---

## Scanning with propecia.c

1. Download and compile:
```bash
# download 
wget --no-check-certificate https://dl.packetstormsecurity.net/UNIX/scanners/propecia.c
# compile 
gcc propecia.c -o propecia
# copy to /bin or other $PATH
sudo cp propecia /bin
```
2. Run:
```bash
propecia 192.168.1 22 # scan subnet for ssh servers
propecia 192.16.1 80 # scan subnet for http servers
propecia 192.16.1 443 # scan subnet for https servers
propecia 192.168.1 3389 # scan subnet for rdp servers
```

<small>*reference: <https://packetstormsecurity.com/files/14232/propecia.c.html>*</small>

---



