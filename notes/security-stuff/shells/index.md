---
layout: default
title: Shells / Reverse Shells
nav_order: 2 
parent: security stuff
has_children: true
permalink: /notes/security-stuff/shells
---

## Quick shells 
1. `python -c 'import pty; pty.spawn("/bin/sh")'`
2. `/bin/sh -i`
3. from within vi: `:!bash` or `:set shell=/bin/bash:shell`
4. from within nmap: `!sh`


## Resources / More info:
- [Escaping binaries - GTFOBins](https://gtfobins.github.io/)


