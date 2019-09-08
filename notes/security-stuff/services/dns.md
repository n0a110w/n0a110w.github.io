---
layout: default
title: Port 53 (DNS)
parent: Enumerating Services
grand_parent: Scanning & Enumeration
nav_order: 2
---

## DNS (Port 53)

1. TOC
{:toc}

### nmap
```bash
# example
nmap -vv --reason -Pn -sV -p53 --script="(dns* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" <target>
```

### fierce
```bash
# example
fierce -dns microsoft.com
```

### dig
```bash
# example
dig axfr example.com @10.10.10.123
```

---
Resources:
- <https://nmap.org/nsedoc/>
- <http://www.zytrax.com/books/dns/>
- <http://cr.yp.to/djbdns/axfr-notes.html>



