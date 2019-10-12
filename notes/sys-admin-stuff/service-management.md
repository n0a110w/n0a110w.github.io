---
layout: default
title: Service Management
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## Systemd
### Listing services
`systemctl` : list running services  
`systemctl --failed` : list failed services  

### Managing Targets (similiar to Runlevels in SysV)
`systemctl get-default` : (dispays the default target for the system)  
`systemctl set-default <target-name>` : (set the default target for the system)  

### Managing Services
`systemctl <start|stop|restart> <service>` : start, stop, or restart a service  
`systemctl reload <service>` : request service to reload its configuration  
`systemctl status <service>` : show current status of service  

### Managing autostart of services
`systemctl is-enabled <service>` : show whether a service is enabled on boot or not  
`systemctl is-active <service>` : show whether a service is currently running (active)  
`systemctl <enable|disable> <service>` : enable/disable a service on boot  

### Masking services
`systemctl mask <service>` : mask a service (makes it hard to start by mistake)  
`systemctl unmask <service>` : unmask a service  

### Restart Systemd
`systemctl daemon-reload`


### Diagnosing problems with a service
`journalctl -xe` : loads the last 1000 logged lines into a pager, jumping to the end  
`journalctl -f` : follow log messages as they come in  
`journalctl -f -t sshd` follow logs for a particular service, in this case "sshd"  
`journalctl -p err -S yesterday` : show all items logged as errors since yesterday  
`tail -f /var/log/messages` : If journalctl is not available, most services on the system log to /var/log/messages  
`tail -f /var/log/secure` : Or, if the service is privileged, it may log sensitive data to /var/log/secure  





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
