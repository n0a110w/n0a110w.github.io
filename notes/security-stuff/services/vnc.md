---
layout: default
title: VNC 
parent: Enumerating Services
grand_parent: security stuff
nav_order: 9
---

## VNC (Port 5900)
### nmap
```bash
# run vnc-related NSE scripts
sudo nmap -Pn -n --open -p5900 --script=realvnc-auth-bypass,vnc-info 192.168.1.1/24
```


