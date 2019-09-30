---
layout: default
title: wpa_cli
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. {TOC}
:{toc}

### in order to use wpa_cli, a control interface must be specified for wpa_supplicant, and it must be given the rights to update the configuration. Do this by creating a minimal configuration file:  
 (arch wiki)[https://wiki.archlinux.org/index.php/WPA_supplicant#Connecting_with_wpa_cli]
```bash
/etc/wpa_supplicant/wpa_supplicant.conf
---
ctrl_interface=/run/wpa_supplicant
update_config=1
```
