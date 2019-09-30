---
layout: default
title: IP Addresses
parent: recon
grand_parent: security-stuff
nav_order: 2
---

There's quite a lot of resources (both free and paid) for gathering information on IP Addresses. Below are my favorite at the time of writing
### [ip-api.com](http://ip-api.com "official site")
```bash
## ip-api.com , free @ 150req/min (api key not required)
## -- will output JSON, CSV, XML, TEXT, PHP, BATCH
## -- supports IPV4, IPV6 and domains
## basic usage: will return colorized json output
curl http://ip-api.com/$IP
```

### [ip-tracer](https://github.com/Rajkumrdusad/IP-Tracer "github repository")
```bash
## basic usage:
ip-tracer -t $IP # run against a target IP (-t, followed by IP)
ip-tracer -m # run against YOUR public IP (-m, omit IP)
```
### [ipinfo.io](https://ipinfo.io/ "official site")
```bash
## basic usage: returns json response
curl https://ipinfo.io/$IP/json

# it is then possible to pipe results to jq for parsing
# i.e.:
curl https://ipinfo.io/$IP/json | jq .city,.postal,.org
```
