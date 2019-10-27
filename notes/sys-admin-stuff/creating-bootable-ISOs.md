---
layout: default
title: Bootable ISOs     
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}


## Build BIOS only ISO (no isohybrid)
```bash
# this is a special case that when booting on an old macmini1,1 (early 2006 model) if there is more than one El Torito boot record on the disc, the firmware gets confused and hangs. most, if not all, distros now include 2 El Torito boot records (one for BIOS boot and one for EFI boot) so are incapable of booting on the old macmini1,1 firmware. In order to workaround this issue, it is necessary to repack the image as a noUEFI, nohybrid ISO.

# below are the commands necessary to generate the correct nonhybrid ISOs for different distros
# devuan
mount -o loop devuan-live.iso /mnt/tmp
mkdir myiso
cp -r /mnt/tmp/* myiso/
umount /mnt/tmp
cd myiso/
xorriso -as mkisofs -r -J -joliet-long -rr -l -partition_offset 16 -V devuan-live -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -o devuani386-nohybrid-live.iso .

# puppy linux
mount -o loop bionicpup32-8.0-uefi.iso /mnt/tmp
cp -r /home/user_name/mnt/* myiso/
umount /mnt/tmp
cd myiso
/usr/bin/mkisofs -o /home/user_name/Bionicpup32-nohybrid.iso -v -J -R -D -A Bionicpup -V Bionicpup -no-emul-boot -boot-info-table -boot-load-size 4 -b isolinux.bin -c isolinux.boot .
```




## Resources and More
{: .no_toc }
