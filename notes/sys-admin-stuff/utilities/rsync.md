---
layout: default
title: rsync
parent: utilities
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
### Links
{: .no_toc }
#### Reference
{: .no_toc }

