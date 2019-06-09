---
layout: default
title: RPC / NFS 
parent: Enumerating Services
grand_parent: security stuff
nav_order: 7
---

## RPC / NFS (Port 111/2049)
> "*Network File System (NFS) is a distributed file system protocol orginally developed by Sun Microsystems in 1984, allow a user on a client computer to access files over a computer network much like local storage is accessed. NFS, like many other protocols, builds on the Open Network Computing Remote Procedure Call (ONC RPC) system. The NFS is an open standard defiend in Request for Comments (RFC), allowing anyone to implement the protocol.*"  
Reference: <https://en.wikipedia.org/wiki/Network_File_System>  

### RPC

```bash
# rpcinfo: makes a call to RPC server and reports what it finds
rpcinfo -p $target

# nmap: same thing but using the rpcinfo script
nmap -Pn -n -T4 -sV --script=rpcinfo -p111 $target
```


### NFS
```bash
# run default scripts with nmap
nmap -sC -p2049 $target

# run specific nfs-related nse scripts
sudo nmap -Pn -n --open -p111 --script=nfs-ls,nfs-showmount,nfs-statfs,rpcinfo 192.168.1.1/24

# if any NFS service is verified, try to export a list of the shares using showmount
showmount -e $target
```


