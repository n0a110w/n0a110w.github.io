---
layout: default
title: find
parent: utilities
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### Find all files owned by ROOT with the SUID bit TRUE

```bash
# find all files owned by root w/ the suid bit set
find / -user root -perm -4000 -print 2>/dev/null

# same thing + prints out the file's permissions and size 
find / -perm -4000 -printf "%m %u %p %kKB\0\n" 2>/dev/null

# same thing as above, but prints the output to a file using `fprintf`  
find / -perm -4000 -fprintf /tmp/suid.txt "%m %u %p %kKB\0\n" 2>/dev/null

# another way
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

```




---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }


