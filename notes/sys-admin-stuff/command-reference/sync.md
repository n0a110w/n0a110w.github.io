---
layout: default
title: sync
parent: command reference 
grand_parent: sys admin stuff
nav_order: 2
---

`sync` is used to synchronize cached writes to persistent storage.  
<small>It is often useful to watch the progress of a sync operation while it's working, here are a few methods depending on the situation:</small>

1. "Dirty" memory (memory waiting to be written back to disk)
```bash
sync & watch -n 1 grep -e Dirty: /proc/meminfo
```
2. System-wide (memory waiting to be written back to disk AND memory which is actively being written back to disk)
```bash
sync & watch grep -e Dirty: -e Writeback: /proc/meminfo
```
3. Device-specific (i.e. SD Card, USB, ..., etc)
```bash
sync & watch -t -n1 'awk "{ print \$9 }" /sys/block/sdd/stat'
```

---

## Resources and More
{: .no_toc }
