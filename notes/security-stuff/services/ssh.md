---
layout: default
title: SSH 
parent: Enumerating Services
grand_parent: security stuff
nav_order: 9
---

## SSH (Port 22)
### nmap
```bash
# run ssh-related nse scripts
sudo nmap -Pn -n --open -p22 --script=sshv1,ssh2-enum-algos 192.168.1.1/24
```


