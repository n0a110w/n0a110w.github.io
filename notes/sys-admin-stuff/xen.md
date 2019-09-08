---
layout: default
title: xen libvirt
parent: sys admin stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}

## How to install Xen Level-1 Hypervisor on Debian Buster

```  
# install xen-hypervisor and xen-tools  
apt-get install xen-hypervisor-4.11-amd64 xen-tools
                                                         
# if you need to, check current version of xen-hypervisor available in the repo with:
apt-cache search xen-hypervisor

# in previous versions of debian, it was necessary to prioritize Xen over native
 linux in /etc/default/grub
# in Buster, this is not required.                                       
``` 


## How to configure bridged networking
### brctl (bridge-utils)
```
# create the bridge
brctl addbr xenbr0

# add interfaces to the bridge
brctl addif xenbr0 enp3s0
```

### /etc/network/interfaces
This is an example of my /etc/network/interfaces file 
```
iface enp3s0 inet manual
auto xenbr0
iface xenbr0 inet dhcp
  bridge_ports enp3s0
```


```
# make the following changes to /etc/default/xendomains , and this will force the VMs to shutdown upon dom0 shutdown instead of clogging up /var with system resume images. 
XENDOMAINS_RESTORE=false
XENDOMAINS_SAVE=""
```




---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }


