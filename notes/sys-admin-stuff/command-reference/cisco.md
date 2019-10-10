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
show running-config | include http

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
#### Show Interfaces + More
```bash
# show all interface information (brief)
show ip interface brief # sh ip int br

# show specific interface
show interface gigabitEthernet 0/1

# bring interface up / down
interface gigabitEthernet 0/1
shut # down
no shut # up

# determine what type of cable is connected to a serial port
show controllers serial 0/0/0
# --- the first few lines of output will help determine the type of cable
# --- for example, DTE V.35 indicates a DTE cable is attached to the interface

# show interface capabilities
sh int fa 1/0/1 cap
```

##### Duplex Settings
- Note: The default duplex setting for a Cisco device (switch) is auto.
```bash
# show an interface's Duplex capabilities
sh int fa 1/0/1 cap | inc Duplex

# show an interface's current duplex setting
sh int status
sh int fa 1/0/1
show running-config
# Note: Routers do not have the same show commands to display interface status as switches do. If you attempt to issue the show interface status command on a router, no output will result.

# change duplex setting to half
conf t
int gigabitEthernet 0/0
duplex half
exit
```

##### Speed Settings
```bash
# show an interface's speed capabilities
sh int fa 1/0/2 cap | inc Speed

# change speed setting
conf t
int gigabitEthernet 0/0
speed ?
speed 1000
exit
```

##### Collisions
- Note: Switches, as opposed to hubs, are used to isolate each port to a collision domain of its own. This essentially eliminates all collisions and makes high-speed Ethernet networks much more efficient. If however collisions are detected on a switch port, it is a sign that something is incorrectly configured or that users are misusing the network resources. Such a situation must be investigated.



#### Assign IPv4 Address to VLAN
```bash
# VLAN - switched virtual interface (SVI)
interface vlan 1
ip address 192.168.10.10 255.255.255.0
no shutdown # no shut
```

#### IPV6
Note: Many of the commands that are used for IPv4 are also used for IPv6. The only difference is that instead of using the keyword 'ip', you use the keyword 'ipv6'
##### Show ipv6 interface information
```bash
show ipv6 interface brief #sh ipv6 int brief
```

##### Enable IPV6 routing globally
```bash
conf t
ipv6 unicast-routing
```

##### Assign IPV6 to interface and enable
```bash
interface gigabitEthernet 0/0
ipv6 address 2001:a:0:1::1/64
ipv6 enable
no shutdown
exit
```

##### Assign IPv6 link local address
```bash
conf t
interface gigabitEthernet 0/0
ipv6 address fe80::1 link-local
exit
```

##### SLAAC
- Stateless Address Auto Configuration
- SLAAC is a functionality of IPv6 that doesn't have a counterpart in IPv4. It is enabled by default on interfaces where IPv6 is enabled so there is no further configuration necessary.
- SLAAC provides global unicast IPv6 addresses to clients from the local router without the use of DHCP
- Note: SLAAC functions using what are called Router Solicitation (RS) and Router Advertisement (RA) messages. The RS is sent by the client asking for addressing information, and the RA is sent as a response from the router. When a router sends an RA where the M and O flags are set to 0, this indicates to the client that SLAAC is available and IPv6 addressing information will be provided.  
```bash
# to ensure SLAAC is enabled on the router, set the M and O flags to "0"
conf t
interface gigabitEthernet 0/0
no ipv6 nd managed-config-flag
no ipv6 nd other-config-flag
exit
# slaac is now explicitly enabled on this interface

# in order to configure a SLAAC client, enable autoconfig on the client interface
conf t
interface gigabitEthernet 0/0
ipv6 address autoconfig
exit
```

### Switching
#### MAC Address Table (CAM Table)
```bash
# show mac address table
sh mac address-table
sh mac address-table dynamic
sh mac address-table static

# determine the maximum size of the MAC address table
sh mac address-table count

# show interface information
sh ip int brief

# assign a STATIC mac address
conf t
mac address-table static FFFF.FFFF.FFFF vlan 1 interface fastEthernet 1/0/2
exit
```

#### Adjust the Aging Timer
- Note: MAC address aging occurs only for dynamically learned MAC addresses. Static entries are never aged out.
- By default, the MAC address aging timer is set to 300 seconds, or five minutes.
```bash
conf t
mac address-table aging-time 10
exit
```

#### Frame Switching Methods
1) Store-and-Forward
  - Store-and-Forward switching does what its name suggests. It receives a frame and stores it in its entirety in the switch buffer before it begins sending it out of its egress port. This allows the switch to read and calculate the Frame Check Sequence which is in the trailer of the frame, to verify that there were no errors in transmission before it sends it out of the egress port. In this scenario, both latency and reliability are increased.
2) Cut-Through
  - Cut-Through switching begins sending a frame out the egress port before it has been received in its entirety. Switching begins once the destination MAC address has been read from the header of the frame and the egress port has been determined. Here, latency is decreased as is reliability.

#### Frame Flooding
- Frame flooding occurs when a frame enters a switch that does not have the destination MAC address within the MAC table. If this is the case, the switch does not know where to send the frame, so it sends it out all ports except the port from which the frame entered the switch. This is frame flooding. If the device with the destination MAC address in question is connected to one of the switch’s ports, it will answer and its MAC address will be added to the MAC table.
- A MAC Flooding Attack can also occur when an attacker fills the maximum number of entries in the MAC table with fudged MAC addresses. After this point, the memory is considered exhausted and any new/legitimate traffic will not be able to have its MAC address stored and therefore the switch will always flood any frames that it receives out of all the interfaces (essentially acting as a hub instead of a switch).
















### Routers
```bash
# display routing protocols
show ip protocols

# show routing table
show ip route

# configure IP address of interface
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shut

# show access list
sh ip access-lists 10
# remove access list from an interface
interface gigabitEthernet 0/1
no ip access-group 10 out

# enable https
ip http secure-server

```





---

## Resources and More
{: .no_toc }
