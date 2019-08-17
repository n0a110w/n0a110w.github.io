---
layout: default
title: misc
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### kill all processes owned by a user
```bash
pkill -9 -u `id -u $username`
```

### Create a super fast ram disk
```bash
mkdir -p /mnt/ram
mount -t tmpfs tmpfs /mnt/ram -o size=8192M
```

### Check if last command was succesful
```bash
if [ $? -eq 1 ] ; then echo failed; else echo success; fi
```


### This is my goto "find" function to locate files
```bash
fil()
  {
    find . name \*${1}\* -ls;
  }

```

---

## Resources and More
{: .no_toc }
