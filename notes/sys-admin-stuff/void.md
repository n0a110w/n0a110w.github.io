---
layout: default
title: template
parent: sys admin stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}

```
# install
KVM Virtualization on Void Linux
sudo xbps-install -S dbus virt-manager libvirt qemu

# enable services
sudo ln -s /etc/sv/dbus /var/service
sudo ln -s /etc/sv/libvirtd /var/service
sudo ln -s /etc/sv/virtlockd /var/service
sudo ln -s /etc/sv/virtlogd /var/service

# make sure your user is part of the libvirt group
sudo usermod -aG libvirt $USER

# launch
virt-manager
```


---

## Resources and More
{: .no_toc }
