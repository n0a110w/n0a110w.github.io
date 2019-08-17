---
layout: default
title: bgp
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## How to monitor and maintain basic BGP
- Basic cli syntax:
```bash
show ip bgp summary
show ip bgp
show ip bgp 7.7.7.7
show ip bgp neighbors
show ip bgp neighbors 12.12.12.1
show ip bgp neighbors 4.4.4.4
clear ip bgp
clear ip bgp 12.12.12.1
```
- It is possible to pass commands directly to vtysh using the `-c` paramater
```bash
sudo vtysh -c 'show ip bgp'
```

---

## Resources and More
{: .no_toc }
