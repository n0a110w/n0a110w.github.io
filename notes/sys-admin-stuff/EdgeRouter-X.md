---
layout: default
title: EdgeRouter-X 
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## General Setup
### Create a new user
```bash
set system login user <newusername> authentication plaintext-pasword <password>
commit
```
- Verify by logging out and logging in to the new account, then delete the default account 'ubnt'
```bash
delete system login user ubnt
commit && save
```

### System Settings
```bash
set system host-name <hostname>
set system domain-name <name.domain>
set system name-server 1.1.1.1
set system time-zone America/New_York
```

### Update the firmware
```bash
add system image https://dl.ubnt.com/firmwares/edgemax/v1.10.x/ER-e50.v1.10.9.5166958.tar
```

### Basic LAN Setup (configure eth2, dns forwarding and dhcp to vlan)
```bash
set interfaces ethernet eth2 address 192.168.10.1/24
set service dhcp-server shared-network-name vlan10 subnet 192.168.10.1/24 default-router 192.168.10.1
set service dhcp-server shared-network-name vlan10 subnet 192.168.10.1/24 dns-server 192.168.10.1
set service dhcp-server shared-network-name vlan10 subnet 192.168.10.1/24 start 192.168.10.10 stop 192.168.10.100
set service dns forwarding listen-on eth2
```

### Basic WAN Setup (configure eth0 via DHCP)
```bash
# setup eth0 to use dhcp 
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 description 'WAN_Internet'

# verify the eth0 IP was acquired from DHCP:
show dhcp client leases

# we also need to setup outbound NAT to translate all internal traffic thru eth0:
set service nat rule 5000 description 'Outbound NAT'
set service nat rule 5000 log disable
set service nat rule 5000 outbound-interface eth0
set service nat rule 5000 protocol all
set service nat rule 5000 type masquerade
```

### Restrict Management Portals (SSH and WebUI)
```bash
set service ssh listen-address 192.168.10.1/24
set service gui listen-address 192.168.10.1/24
```

### Basic Firewall Configuration
<small>- `WAN_IN`: matches on established/related and invalid traffic that is passed thru the router (WAN TO LAN)</small> 
<small>- `WAN_LOCAL`: matches on established/related and invalid traffic that is destined for the router itself (WAN to LOCAL)</small>

#### WAN to Internal
```bash
set firewall name WAN_IN default-action drop
set firewall name WAN_IN description 'WAN to internal'

set firewall name WAN_IN rule 10 action accept
set firewall name WAN_IN rule 10 description 'Allow established/related'
set firewall name WAN_IN rule 10 state established enable
set firewall name WAN_IN rule 10 state related enable

set firewall name WAN_IN rule 20 action drop
set firewall name WAN_IN rule 20 description 'Drop invalid state'
set firewall name WAN_IN rule 20 state invalid enable

set interfaces ethernet eth0 firewall in name WAN_IN
```

#### WAN to Router 
```bash
set firewall name WAN_LOCAL default-action drop
set firewall name WAN_LOCAL description 'WAN to router'

set firewall name WAN_LOCAL rule 10 action accept
set firewall name WAN_LOCAL rule 10 description 'Allow established/related'
set firewall name WAN_LOCAL rule 10 state established enable
set firewall name WAN_LOCAL rule 10 state related enable

set firewall name WAN_LOCAL rule 20 action drop
set firewall name WAN_LOCAL rule 20 description 'Drop invalid state'
set firewall name WAN_LOCAL rule 20 state invalid enable

set interfaces ethernet eth0 firewall local name WAN_LOCAL
```

## Enable Hardware Offloading
<small>*reference: <https://help.ubnt.com/hc/en-us/articles/115006567467-EdgeRouter-Hardware-Offloading#3>*</small>
- type the following commands at the terminal to enable ipsec and hwnat offloading
```bash
configure
set system offload hwnat enable
set system offload ipsec enable
commit
save
```
- check that offloading was enabled
```bash
show ubnt offload
```

## Configure as OpenVPN Server
<small>*reference: <https://help.ubnt.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server>*</small>
### Generate certificate + keys
1. Generate root certificate   
```bash
# check the date/time before beginning
show date
# switch to root
sudo su
# generate a diffie-hellman key file and place it in /config/auth/
openssl dhparam -out /config/auth/dh.pem -2 2048
# generate a root certificate
cd /usr/lib/ssl/misc
./CA.sh -newca
# copy the cert+key to /config/auth/
cp demoCA/cacert.pem /config/auth
cp demoCA/private/cakey.pem /config/auth
```

2. Generate server certificate
```bash
# generate the server certificate
./CA.pl -newreq
# sign the server certificate
./CA.pl -sign
# move and rename the server cert+key files to /config/auth/
mv newcert.pem /config/auth/server.pem
mv newkey.pem /config/auth/server.key
```

3. Generate client certificate
```bash
# generate, sign and move the certificate+key files for a client
./CA.pl -newreq
common name: should be client name :)
./CA.pl -sign
mv newcert.pem /config/auth/client1.pem
mv newkey.key /config/auth/client1.key
```

4. Remove passwords from the client and server key files
```bash
openssl rsa -in /config/auth/server.key -out /config/auth/server-no-pass.key
openssl rsa -in /config/auth/client1.key -out /config/auth/client1-no-pass.key
mv /config/auth/server-no-pass.key /config/auth/server.key
mv /config/auth/client1-no-pass.key /config/auth/client1.key
# add read permissions for non-root users to the client key
chmod 644 /config/auth/client1.key
```

5. That's it! Copy all necessary client files (client1.key, client1.pem and cacert.pem)

6. Create a client ovpn file from the template below  
```bash
client
dev tun
proto udp
remote <server-ip> 1194
float
resolv-retry infinite 
nobind
persist-key 
persist-tun 
verb 3
ca cacert.pem 
cert client1.pem
key client1.key
```

### interface and firewall rules 
```bash
# add a firewall rule for the openvpn traffic to the WAN_LOCAL firewall policy
set firewall name WAN_LOCAL rule 30 action accept
set firewall name WAN_LOCAL rule 30 description openvpn
set firewall name WAN_LOCAL rule 30 destination port 1194
set firewall name WAN_LOCAL rule 30 protocol udp

# configure the openvpn virtual tunnel interface
set interfaces openvpn vtun0 mode server
set interfaces openvpn vtun0 server subnet 172.16.1.0/24
set interfaces openvpn vtun0 server push-route 192.168.1.0/24
set interfaces openvpn vtun0 server name-server 192.168.1.1

# link the server certificate+keys to the virtual tunnel interface
set interfaces openvpn vtun0 tls ca-cert-file /config/auth/cacert.pem
set interfaces openvpn vtun0 tls cert-file /config/auth/server.pem
set interfaces openvpn vtun0 tls key-file /config/auth/server.key
set interfaces openvpn vtun0 tls dh-file /config/auth/dh.pem

# add the virtual tunnel interface to the DNS forwarding interface list
set service dns forwarding listen-on vtun0

# commit and save
commit ; save
```

## Configure as OpenVPN Client

- copy the ovpn file to /config/auth/ and then perform the following commands:
```bash
set interfaces openvpn vtun1 config-file /config/auth/router.ovpn
set interfaces openvpn vtun1 description 'VPN Out'
```
  - Now the interface vtun1 should be listed in the WebUI   
- Next, perform the following to setup host-specific access to the VPN tunnel:
1. Add the line "route no-pull" to the bottom of your ovpn file: 
```bash
echo "route-nopull" >> router.ovpn"
```
2. Add a NAT rule with vtun1 as outbound interface. Source address will be the host IP followed by /32 bitmask. (i.e. 192.168.10.101/32)
```bash
# NAT rules can also be 'more easily' accomplished using the WebUI
set service nat rule 5100 description 'Outbound NAT to VPN'
set service nat rule 5100 log enable
set service nat rule 5100 outbound-interface vtun1
set service nat rule 5100 source address 192.168.10.101/32
set service nat rule 5100 protocol all
set service nat rule 5100 type masquerade
commit && save
```
3. Create a static route using vtun1 interface as next hop
```bash
set protocols static table 1 interface-route 0.0.0.0/0 next-hop-interface vtun1
```
4. Create firewall modify rule for each host you want to route through the vpn
```bash
set firewall modify OPENVPN_ROUTE rule 10 description 'traffic from pc to vtun1'
set firewall modify OPENVPN_ROUTE rule 10 source address 192.168.10.101/32
set firewall modify OPENVPN_ROUTE rule 10 modify table 1
```
5. Apply the firewall modify rule "in" to your LAN interface. This example is applied in interface switch0:
```bash
set interfaces switch switch0 firewall in modify OPENVPN_ROUTE
```
6. FINISHED!

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }
- [EdgeOS User Guide](]https://dl.ubnt.com/guides/edgemax/EdgeOS_UG.pdf)
- [How to Create a WAN Firewall Rule](https://help.ubnt.com/hc/en-us/articles/204962154-EdgeRouter-How-to-Create-a-WAN-Firewall-Rule)
- [Configure ER-X as OpenVPN Server](https://help.ubnt.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server)
- [Enable hardware offloading](https://help.ubnt.com/hc/en-us/articles/115006567467-EdgeRouter-Hardware-Offloading#3)









