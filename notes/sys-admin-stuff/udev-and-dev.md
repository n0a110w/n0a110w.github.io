---
layout: default
title: udev and dev
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## udev
udev is responsible for dynamic device management needed for hot plugging devices.
Information about configured and active devices is contained in the /dev virtual filesystem.
- When a device changes state, the kernel sends an event to the systemd-udevd.service daemon.
  - The daemon searches configured rules to match the event
  - The udev rules are stored and read from the following locations: (check the `udev` manpage on your system)
    - /lib/udev/rules.d (system directory)
    - /run/udev/rules.d (volatile runtime directory)
    - /etc/udev/rules.d (local administration directory)
- If you want to monitor udev events, use the command `udevadm`. 

## /dev
- the /dev virtual filesystem describes the devices on your system.
- The value in the first column represents the type of device it is:
  - b : block device (hdd)
  - c : character device (terminal, printer, special)
  - d : directory
  - l : symbolic link



## Resources and More
{: .no_toc }
