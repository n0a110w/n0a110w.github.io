---
layout: default
title: Boot Managers
parent: sys admin stuff
nav_order: 2
---
 1. TOC
{:toc}

Notes on Boot Managers...

## GRUB
Grand Unified Boot Loader (GRUB) <small>replaced by GRUB2</small>
- menu.lst
- grub.cfg
- grub.conf
- grub-install
- grub2-install
- grub-mkconfig

---

## GRUB2
Grand Unified Boot Loader 2 (GRUB2)
- /etc/default/grub
- /etc/grub.d/
- the grub2-mkconfig command generates a new GRUB2 configuration
- the /boot/grub2/grub.cfg will be updated
- cat /boot/grub2/device.map
- grub2-install /dev/sda
- grub2-mkconfig -o /boot/grub2/grub.cfg



## LILO
Linux LOader (LILO)

---

## Resources and More
{: .no_toc}
### Links
{: .no_toc}
#### Reference
{: .no_toc}
[LILO - arch wiki](https://wiki.archlinux.org/index.php/LILO)  
[LILO - slackdocs](https://docs.slackware.com/slackbook:booting)

[GRUB - arch wiki](https://wiki.archlinux.org/index.php/Grub)  
[GRUB vs GRUB2 - ubuntu help](https://help.ubuntu.com/community/Grub2#GRUB_vs_GRUB_2)
