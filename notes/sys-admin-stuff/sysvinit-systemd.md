---
layout: default
title: SysVInit VS Systemd
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## Service Management

| SysVInit | Systemd | Notes |
|:--------- |:-------- |:------ |
| `service <service> <start,stop,restart,reload>`<br />Example: `service sshd start` | `systemctl <start,stop,restart,reload> <service>`<br />Example: `systemctl start sshd` | Used to control a service (not reboot persistent) |
| `service <service> condrestart`| `systemctl condrestart <service>`| Restarts a service already running |
| `service <service> status` | `systemctl status <service>` | Is the service running? |
| `ls /etc/rc.d/init.d/` | `systemctl list-unit-files --type=service`<br />`ls/ /lib/systemd/system/*service`<br />`ls /etc/systemd/system/*service` | Used to list services that can be started or stopped with scripts |
| `chkconfig <service> <on,off>` | `systemctl <enable,disable> <myservice>` | Turn service on, start at next boot or other trigger |
| `chkconfig <service>` | `systemctl is-enabled <service>` | Check whether a service is configured to start on boot or not |
| `chkconfig --list` | `systemctl list-unit-files --type=service`<br />`ls /etc/systemd/system/*.service` | List all services and runlevels and what they are configured for |
| `chkconfig <service> --list` | `ls /etc/systemd/system/*wants/<service>.service` | List all runlevels for which this service is configured for |
| `chkconfig <service> --add` | `systemctl daemon-reload` | Used when you create a new service file or modify any configuration | 


---

## Runlevels

| SysVInit | Systemd | Notes |
|:--------- |:-------- |:------ |
| `0` | `poweroff.target` | halt the system |
| `1` | `rescue.target` | single-user mode |
| `2` | `multi-user.target` | multi-user.target without networking |
| `3` | `multi-user.target` | multi-user.target with networking |
| `4` | `multi-user.target` | user-configurable |
| `5` | `graphical.target` | multi-user.target with gui |
| `6` | `reboot.target` | reboot the system |


---

## Resources and More
{: .no_toc}
### Links
{: .no_toc}
#### Reference
{: .no_toc}



