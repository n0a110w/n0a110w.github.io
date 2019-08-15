layout: default
title: ip - command reference
parent: sys admin stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}

# How to add/remove an IP address to interface (volatile)
```bash
# adding an address
ip addr add 172.16.0.10/24 dev eth2
# removing an address
ip addr del 172.16.0.10/24 dev eth2
```

# How to enable/disable an interface
```bash
# enabling an interface
ip link set eth2 up
# disabling an interface
ip link set eth2 down
```

# How to display route table
```bash
ip route show
```

# How to add/remove static routes
```bash
# adding a static route
ip route add 10.10.10.0/24 via 192.168.0.1 dev eth0
# removing a static route
ip route del 10.10.10.0/24
# adding a default gateway
ip route add default via 192.168.0.1
```

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }
