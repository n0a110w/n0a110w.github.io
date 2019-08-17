---
layout: default
title: dd
parent: command reference 
grand_parent: sys admin stuff
nav_order: 2
---

A couple of nifty things with dd
Be cautious, for example the `conv=sync,noerror` options should be considered essential

# create a backup image of disk
```bash
# make a backup of disk to image file
dd conv=sync,noerror status=progress bs=512 count=21459327 if=/dev/sdX of=/sdX-Backup-512k-blocks.img.gz
# if status=progress option is unavailble, you can still get progress
# by sending USR1 to the running dd process.
# i.e.:
kill -USR1 <PID>

# to save space, compress the data with gzip:
dd bs=512 count=31459327 if=/dev/sdX | gzip -c > /sdX-Backup-512k-blocks.img.gz
# and to restore the image:
gunzip -c /image.img.gz | dd of=/dev/sdX


# another trick is to pipe `if` to `of` like so
# (has been reported to increase throughput)
dd if=/dev/sdX | of=/dev/sdb
```

# check write speeds
```bash
sync && dd if=/dev/zero of=./tempfile bs=1M count=1024 && sync
```

---

## Resources and More
{: .no_toc }
