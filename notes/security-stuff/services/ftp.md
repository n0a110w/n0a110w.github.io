---
layout: default
title: Port 21 (FTP)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 3
---

## FTP (Port 21)

1. TOC
{:toc}

### nmap
```bash
# run ftp-related nse scripts
nmap -vv --reason -Pn -sV -p21 --script="(ftp* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" <target>
```

### Bruteforce Logins
#### Hydra
```bash
hydra -L <user-wordlist> -P <password_wordlist> -e nsr -s 21 ftp://target.com
```

#### Medusa
```bash
medusa -U <user-wordlist> -P <password_wordlist> -e ns -n 21 -M ftp -h <target>
```


