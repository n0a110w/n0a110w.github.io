---
layout: default
title: procfs
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## procfs
procfs (mounted at /proc) is a virtual file system that provides insight into kernel data structures and allows some of the be changed on the fly. The parameters that you can control in /proc can also be controlled via the `sysctl` command.  
You can use the command `lsdev` (from package: `procinfo`) which simplifies the presentation of the information in /proc   

- Good to Knows:
  - /proc/$$
  - /proc/$fish_pid
  - /proc/interrupts
  - /proc/dma
  - /proc/ioports
  - /proc/cpu






## Resources and More
{: .no_toc }
