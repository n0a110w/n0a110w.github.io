---
layout: default
title: rsync
parent: command reference 
grand_parent: sys admin stuff
nav_order: 2
---

```
# backup an entire system with rsync
# ignoring virtual vilesystems and mounts
# rsync -aAXv / --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} /mnt/nfs/

```

---

## Resources and More
{: .no_toc }
