---
layout: default
title: nmap
parent: Scanning
grand_parent: security stuff
nav_order: 2
---
1. TOC
{:toc}

## Basic Syntax
```bash
nmap <options> <port> <target>
```

## Target Specification <target>
IPv4 address: 192.168.1.1
IPv6 address: AABB:CCDD::FF%eth0
Host Name: target.name
IP address range: 192.168.1.0-255
CIDR Block: 192.168.1.1/24
Use list of targets from file: -iL <filename>

## Port Specification <port>
| `-F` | top 100 most popular ports |
| `--top-ports <n>` | scan *n* of most popular ports |
|`-p80,443,3389` | list of ports |
|`-p1-65535` | range of ports |
|`-pU:53,T:445` | list of ports by protocol | 
|`-r` | scan linearly (1,2,3,...,65535 |
|`-p-` | scan all ports 1-65535` |

## Scan Types
* `-sn` : ping scan only, skip port scan
* `-sS`         : TCP SYN Scan
* `-sT`         : TCP Connect Scan
* `-sN`         : TCP Null Scan
* `-sF`         : TCP FIN Scan
* `-sA`         : TCP ACK Scan
* `-sU`         : UDP Scan
* `-sV`		: Version Scan
* `-sO`         : IP Protocol Scan
* `-O`		: OS detection
* `-A`		: aggressive scan (enables OS and version detection, script scanning and traceroute)

## Other Options
* `-T<0-5>`		: timing (higher is faster)
* `-Pn`         : skip host discovery
* `-v(vv)`               : verbose output
* `-R`          : force reverse DNS resolution
* `-n`          : disable reverse DNS resolution
* `-6`		: IPv6 scan

## Probe Options
* `-PS`         : TCP SYN Ping
* `-PA`         : TCP ACK Ping
* `-PU`         : UDP Ping
* `-PY`         : SCTP Init Ping
* `-PE`         : ICMP Echo Ping
* `-PP`         : Timestamp Ping
* `-PM`         : Address Mask Ping
* `-PO`         : Protocol Ping
* `-PR`         : ARP Ping

## Aggregate Timing Options
* `T0`		: Paranoid; very slow, used for IDS evasion
* `T1`		: Sneaky; quite slow, also used for IDS evasion
* `T2`		: Polite; slows down to consume less bandwidth
* `T3`		: Normal; default based on target responsiveness
* `T4`		: Aggressive; assumes a fast and reliable network
* `T5`		: Insane; very aggressive, may overwhelm targets or miss open ports
* `--max-retries <#>` : Max number of port scan probe retransmissions
* `--host-timeout <time>` : Give up on target after this long (time in seconds)
* `--min-rate <#> : Send packets no slower than <#> per second
* `--max-rate <#> : Send packets no faster than <#> per second
### Interactive Options (while nmap is running)
* press `d` to increase the debugging level
* press `D` to decrease the debugging level


## Firewall Evasion
* `-f`			: fragment packets
* `-S <ip>` : spoof src ip
* `-g <port#>` or `--source-port` : spoof src port
* `-D <ip>,<ip>` : decoys
* `--spoof-mac <mac>` : spoof src mac
* `--data-length <size>` : append random data
* `--scan-delay <time>` : adjust delay between probes

## Output Formats
* `-oN <file>`		: standard nmap format
* `-oG <file>`		: greppable format
* `-oX <file>`		: XML format
* `-oA <file>`		: output each(all) of the above 

## Scripting Engine (NSE) 
* `--script-updatedb`	: update the script database
* `-sC`		: run default scripts
* `--script=<name>` : run script where <name> is the name of the script
* `--script=<category>	: run group of scripts where <category> is equal to all, auth, default, discovery, external, intrusive, malware, safe or vuln 
* `--script-args=arg1=va1` : append script arguments as necessary

## Examples
### Scanning a subnet for live hosts
```bash
# this will output all found hosts to a textfile
nmap -F -oG - 192.168.1.1/24 | awk '/open/{print $2}' > target-hosts.txt
# ftp servers
nmap -Pn -p21 -oG - iL fastscan.txt | awk '/open/{print $2}' > ftp-hosts.txt
# ssh servers
nmap -Pn -p22 -oG - iL fastscan.txt | awk '/open/{print $2}' > ssh-hosts.txt
# http servers
nmap -Pn -p80,443 -oG - iL fastscan.txt | awk '/open/{print $2}' > http-hosts.txt
# ftp, ssh, http, mssql, rdp servers
nmap -Pn -sV -T4 -oG - -p 21,22,80,443,1433,3389 192.168.1.1/24 | awk '/open/{print $2}' > target-hosts.txt
```
### Run a SYN (stealth) scan
```bash
# also known as a half-open scan because there is no completion of the three-way handshake. Can help avoid IDS
nmap -sS -T4 $target
```
### Run a full-open (TCP connect) scan
```bash
# also known as the TCP connect and full connect scan. it runs the three-way handshake on all ports. easy to detect
nmap -sT -T4 $target
```
### XMAS scan
```bash
# will not work on windows because windows is not RFC 793 compliant
nmap -sX -T4 $target
```
### ACK scan
```bash
# an ACK packet is sent and header reviewed for RST packet TTL 64< (helps detect firewalls)
nmap -sA -T4 $target
```
### UDP Scan
```bash
# slower than a TCP scan
nmap -sU -T4 $target
# can also be paired with a normal TCP scan with a particular port per protocol
nmap -sU -sT -p U:$port,T:$port $target
```

### Intense scan
```bash
nmap -A -T4 $target
# -A : enables OS detection, version detection, script scanning and traceroute
# -T4 : enables aggressive timing
```




## NSE (Nmap Scripting Engine)

Scripts can be invoked by name or category. (categories = all, auth, default, discovery, external, intrusive, malware, safe or vuln)
<small>*Reference: <https://nmap.org/nsedoc/>*</small>  
- By default, nmap stores scripts at `/usr/share/nmap/scripts/`  
- A sample custom script is located at `/usr/share/nmap/scripts/intro-nse.nse`  

***EXAMPLES***:
```bash
# Get help info on a particular script:
nmap --script-help=$script-name

# Enumerate all ssh2 supported algorithms and ciphers:
nmap -sV --script ssh2-enum-algos -p22 $target

# Fingerprint the SSH server key:
nmap -sV --script ssh-hostkey -p22 $target --script-args ssh_hostkey=full

# Scan using default safe scripts:
nmap -sV -sC $target

# Scan for well known service vulnerabilities: (Note: this NSE script can be found at <https://github.com/vulnersCom/nmap-vulners>)
nmap -sV --script vulners $target

# Scan using all scripts in the category "vuln":
nmap -sV --script vuln $target

# Scan using a set of scripts (using wildcard):
nmap -sV --script smb* $target
```

### HTTP Screenshot (.nse) - takes a screenshot of target webservers

- Download and install the http-screenshot.nse script from github:  
<small>*Credit: <https://github.com/SpiderLabs/Nmap-Tools>*</small>
```bash
# clone from github
git clone git://github.com/SpiderLabs/Nmap-Tools.git
# copy the script to the default nmap scripts directory
sudo cp Nmap-Tools/NSE/http-screenshot.nse /usr/share/nmap/scripts/
```

- Perform a scan with the http-screenshot script enabled:
```bash
nmap -Pn -T4 -p80 -A --script=http-screenshot -iL target-ip-list.txt
```
   - OPTIONAL: It is helpful to compile all of the screenshots of the servers in a network into one HTML page for easier viewing. This can be done with the following script:
```bash
> vi screenshot.sh
    #!/bin/bash
    printf "<HTML><BODY><BR>" > port-80-screenshots.html
    ls -1 *.png | awk -F : '{ print $1":"$2"\n<BR><IMG SRC=\""$1"%3A"$2"\" width=400><BR><BR>"}' .>> port-80-screenshots.html
    printf "</BODY></HTML>" >> port-80-screenshots.html
> chmod u+x screenshot.sh
> ./screenshot.sh
# that's it! Now you can serve the HTML page to yourself
```

---






