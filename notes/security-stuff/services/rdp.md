---
layout: default
title: RDP 
parent: Enumerating Services
grand_parent: security stuff
nav_order: 9
---

## RDP (Port 3389)
### nmap
```bash
# run rdp-related nse scripts
sudo nmap -Pn -n --open -p3389 --script=rdp-vuln-ms12-020,rdp-enum-encryption 192.168.1.1/24
```


