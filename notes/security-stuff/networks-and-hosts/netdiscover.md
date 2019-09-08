---
layout: default
title: netdiscover 
parent: Network and Host Discovery
grand_parent: security stuff
nav_order: 2
permalink: /notes/security-stuff/scanning-and-enumeration/networks-and-hosts/netdiscover
---

## Scanning with netdiscover
netdiscover is a tool that sends out ARP requests the same way that a switch or router does in order to discover live hosts

### Basic Usage:
```bash
netdiscover -i eth0 -r 192.168.1.0/24
```
### Fast scan (search for gateways):
```bash 
netdiscover -i eth0 -f
```
### Options:
```
    -i : specify the network interface to sniff at and inject packets
    -r : scan a given range
    -f : enables fastmode scan, only searches for hosts .1 .100 and .254
```
More info on github: <https://github.com/alexxy/netdiscover>

---


