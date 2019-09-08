---
layout: default
title: Port 3306 (MYSQL)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 5
---

## MySQL (Port 3306)

1. TOC
{:toc}

### nmap
```bash
# run mysql-related nse scripts
nmap -vv --reason -Pn -sV -p3306 --script="(mysql* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" <target>
```


