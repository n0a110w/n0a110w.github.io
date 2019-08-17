---
layout: default
title: tcpdump
parent: command reference 
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}


### Capture traffic on remote system with tcpdump and view in Wireshark locally
<small>Instead of copying pcap files from system to system for analysis, it is possible to run tcpdump on a remote host and feed the capture to wireshark over the SSH connection in near real-time.</small>
```bash
ssh root@remotesystem 'tcpdump -s0 -c 65535 -nn -w - not port 22' | wireshark -k -i -
```



---

## Resources and More
{: .no_toc }
- [tcpdump examples](https://hackertarget.com/tcpdump-examples/)
