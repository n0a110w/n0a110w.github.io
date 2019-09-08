---
layout: default
title: Port 554 (RTSP)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 2
---

## RTSP (Port 554)

### nmap
```bash
# run rtsp-url-brute.nse to discover alternative streams (possibly unauthenticated)
nmap -sV -p554 --script rtsp-url-brute <target>
# note , rtsp-url-brute uses the dictionary "nselib/data/rtsp-urls.txt" by default
# can be changed by specifying as an argument

# discover valid rtsp methods with rtsp-methods.nse
nmap -sV -p554 --script=rtsp-methods <target>
```

---

Resources:
- <https://nmap.org/nsedoc/scripts/rtsp-url-brute.html>
- <https://nmap.org/nsedoc/scripts/rtsp-methods.html>


---
