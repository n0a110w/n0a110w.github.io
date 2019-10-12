---
layout: default
title: ps
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### Process monitoring
```bash
# list all processes in a hierchy
ps -e -o pid,user,args --forest

# list process sorted by %cpu usage (the process using %0.0 are disregarded from the output)
ps -e -o pcpu,cpu,nice,state,cputime,args --sort pcpu | sed '/^ 0.0 /d'

# list processes sorted by mem usage (KB)
ps -e -orss=,args= | sort -b -k1,1n | pr -TW$COLUMNS

# list all threads for a particular process
ps -C firefox -L -o pid,tid,pcpu,state

# gather information related to a specific process using `lsof` to list paths that the pid has open
lsof -p $$

# or based on path, list processes this have specified path open
lsof ~
```

---

## Resources and More
{: .no_toc }
