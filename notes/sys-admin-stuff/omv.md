---
layout: default
title: OMV
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## Create a backup image of an OpenMediaVault system
<small>This can be setup as a cronjob and is safe to use on a running system</small>
```bash
# in this example, /dev/sdg represents the OMV system
dd bs=512 count=31459327 if=/dev/sdg | gzip -c > OMV-Backup-512k-blocks.img.gz
```


---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }


