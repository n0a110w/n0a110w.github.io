---
layout: default
title: weechat notes
parent: sys admin stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}


## How to configure server settings in weechat
### add server
`/server add freenode chat.freenode.net/6697 -ssl`
### enable autoconnect
`/set irc.server.freenode.autoconnect on`
### auto-join channels on connection to server
`/set irc.server.freenode.autojoin "#channel1,#channel2,#channel3`
### change default nicks
`/set irc.server_default.nicks comma,separated,list,of,nicks`

## How to configure identity (SASL) in weechat
1. generate key  
`openssl ecparam -genkey -name prime256v1 -out ~/.weechat/ecdsa.pem`
2. get the public key encoded as base64  
```bash
openssl ec -noout -text -conv_form compressed -in ~/.weechat/ecdsa.pem | grep '^pub:' -A 3 | tail -n 3 | tr -d ' \n:' | xxd -r -p | base64
```
3. connect to the server and identify yourself  
`/connect freenode`  
`/msg nickserv identify <password>`
4. set the pubkey  
`/msg nickserv set pubkey <pubkey>
5. configure the SASL options in the server  
`/set irc.server.freenode.sasl_mechanism ecdsa-nist256p-challenge`  
`/set irc.server.freenode.sasl_username "your_nickname"`  
`/set irc.server.freenode.sasl_key "%h/ecdsa.pem"`
6. reconnect to the server and verify the server automatically identifies you  
`/reconnect freenode`

Above was my old method, now I use a dedicated raspberry pi2 as a irc bouncer
After the ZNC server is configured, this is how to connect from a weechat client
```
# How to add and connect to a ZNC server
/server add server server/6697 -username=username/network -password=password
/connect server
/save
 ```

---

## Resources and More
- <https://www.weechat.org/files/doc/stable/weechat_user.en.html#irc_sasl_authentication>
- <https://weechat.org/files/doc/stable/weechat_quickstart.en.html>
- <https://wiki.archlinux.org/index.php/WeeChat>
- <https://wiki.znc.in/Weechat>
