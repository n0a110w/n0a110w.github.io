---
layout: default
title: wpa supplicant
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### basic (empty) configuration file
```bash
# /etc/wpa_supplicant/wpa_supplicant.conf
ctrl_interface=/run/WPA_supplicant
update_config=1
```

### starting wpa_supplicant
```bash
# manually starting wpa_supplicant with specific configuration file
wpa_supplicant -B -i $INTFC -c /etc/wpa_supplicant/wpa_supplicant.conf

# it is possible to enable the dhcpcd->wpa_supplicant hook by creating this symlink:
ln -s /usr/share/dhcpcd/hooks/10-wpa_supplicant /usr/lib/dhcpcd/dhcpcd-hooks/
# after enabled, just executing dhcpcd will automatically launch wpa_supplicant if a valid configuration file exists
dhcpcd
```

### wpa_passphrase
```bash
# wpa_passphrase generates the appropriate network configuration stanza for a target SSID with encryption
# below will generate and append the stanza to the wpa_supplicant configuration file
wpa_passphrase "$SSID" "$PSK" | tee -a /etc/wpa_supplicant/wpa_supplicant.conf
```
