---
layout: default
title: Port 1433 (MS-SQL)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 4
---

## MS-SQL (Port 1433)

1. TOC
{:toc}

### nmap
```bash
# run mssql-related nse scripts
nmap -vv --reason -Pn -sV -p1433 --script="(ms-sql* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" --script-args="mssql.instance-port=1433,mssql.username=sa,mssql.password=sa" <target>
```

### sqsh
```bash
# interactive database shell
sqsh -U <user> -P <password> -S <target>:<port>
```



