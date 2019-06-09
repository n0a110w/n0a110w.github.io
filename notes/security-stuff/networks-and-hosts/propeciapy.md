---
layout: default
title: propecia.py 
parent: Network and Host Discovery
grand_parent: security stuff
nav_order: 2
---

## Scanning with propecia.py

### Download and execute:
```bash
# download from github
git clone https://github.com/Wh1t3Rh1n0/propecia.py.git
# change dir and execute with python2
cd propecia.py
python2 propecia.py 192.168.1 22 # scan subnet for ssh servers
python2 propecia.py 192.168.1 80 # scan subnet for http servers
python2 propecia.py 192.168.1 443 # scan subnet for https servers 
python2 propecia.py 192.168.1 3389 # scan subnet for rdp servers
```

<small>*Reference: <https://github.com/Wh1t3Rh1n0/propecia.py>*</small>

---

