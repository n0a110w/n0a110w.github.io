---
layout: default
title: eMail Recon
parent: recon
grand_parent: security-stuff
nav_order: 2
---

### [emailrep.io](https://emailrep.io/ "official site")
- See also on [github](https://github.com/sublime-security/emailrep.io "github repository")
```bash
## basic usage:
curl https://emailrep.io/$IP
```

### [h8mail](https://github.com/khast3x/h8mail "github repository")
- "requires" API Keys for:
  - [HaveIBeenPwned](https://haveibeenpwned.com/API/Key?ref=h8mail "official site")
  - [hunter.io](https://hunter.io/users/sign_up?utm_medium=api?ref=h8mail "official site")
  - [Snusbase](https://snusbase.com/?ref=h8mail "official site")
  - [WeLeakInfo](https://weleakinfo.com/?ref=h8mail "official site")
  - [Leak-Lookup](https://leak-lookup.com/?ref=h8mail "official site")

```bash
## basic usage:
h8mail -t test@example.com
```
