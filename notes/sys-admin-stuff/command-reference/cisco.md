---
layout: default
title: cisco
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## cisco IOS commands
### GENERAL
#### system info
```bash
# show all system information
show version
```
#### disable interrupting syslog messages
```bash
line con 0
logging synchronous
```
#### password management
```bash
# set password (cleartext)
enable password <password>
# set password (encrypted, md5)
enable secret <password>
# --- NOTE: The Cisco IOS will not allow you to have the same text string for both cleartext and encrypted passwords.
# --- the cleartext passwords will be disallowed if encrypted passwords exist in the same config
#
# encrypt existing passwords in the config
service password-encryption

```
#### config management
```bash
# show running configuration (RAM)
show running-config # show run
# search running-config for parameters
show running-config | include enable password

# show startup configuration (NVRAM)
show startup-config

# copy running config to startup configuration
copy running-config startup-config

# delete the startup configuration
erase startup-config
# or
write erase # wr er
```



### Remote Access
#### telnet, basic
```bash
# there are up to 16 VTYs, usually
# set a password on all at the same time
line vty 0 15
password <password>
login
```
#### ssh, encrypted
```bash
# enable SSH
# --- NOTE: A domain must be configured before the RSA key is generated
ip domain-name example.command
crypto key generate rsa usage-keys
# keep in mind that SSH 1.99 indicates backwards compatibility
# it is good practice to set the SSH version to 2 to disable backwards compatibility
ip ssh version 2
show ip ssh

# configure vty lines to accept SSH
line vty 0 15
transport input ssh
# --- NOTE: enabling ssh disables telnet automatically :)
# --- if you want ssh AND telnet enabled perform the following:
line vty 0 15
transport input telnet ssh

# configure username and password
username <user> privilege 15 secret <password> # 15 is highest access level
# configure the vty lines to use the local user database for login
vty 0 15
login local

# configure MOTD, because why not
banner motd ^n0a110w woz here^
```

### Network
#### Display Interfaces + More
```bash
# show all interface information (brief)
show ip interface brief # sh ip int br

# determine what type of cable is connected to a serial port
show controllers serial 0/0/0
# --- the first few lines of output will help determine the type of cable
# --- for example, DTE V.35 indicates a DTE cable is attached to the interface
```
#### Assign IPv4 Address to VLAN
```bash
interface vlan 1
ip address 192.168.10.10 255.255.255.0
no shutdown # no shut
```

### Routers
```bash
# display routing protocols
show ip protocols

# configure IP address of interface
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shut
```





---

## Resources and More
{: .no_toc }
