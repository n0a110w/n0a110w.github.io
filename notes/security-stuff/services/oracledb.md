---
layout: default
title: OracleDB 
parent: Enumerating Services
grand_parent: security stuff
nav_order: 8
---

## ORACLE DB (Port 1521)
### nmap
```bash
# run oracle-related nse scripts
sudo nmap -Pn -n --open -p1521 --script=oracle-sid-brute --script oracle-enum-users --script-args oracle-enum-users.sid=ORCL,userdb=orausers.txt 192.168.1.1/24
```



