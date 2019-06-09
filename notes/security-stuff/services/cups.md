---
layout: default
title: Port 631 (CUPS / IPP)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 2
---

## CUPS / IPP (Port 631)

### nmap
```bash
# run all cups-related nse scripts
nmap -sV -p631 --script=cups* <target>

# lists printers managed by the CUPS printing service
nmap -sV -p631 --script=cups-info <target>

# lists currently queed print jobs of the remote CUPS service grouped by printer
nmap -sV -p631 --script=cups-queue-info <target>
```

---

Resources:
- <https://nmap.org/nsedoc/scripts/cups-info.html>

---



