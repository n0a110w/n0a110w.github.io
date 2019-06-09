---
layout: default
title: Port 389 (LDAP)
parent: Enumerating Services
grand_parent: security stuff
nav_order: 2
---

## LDAP (Port 389)

1. TOC
{:toc}

### nmap
```bash
nmap -sV -p389 --script="(ldap* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" $target
```
> **Remember:** To see exactly which scripts are being selected to run, pass the expression to `--script-help` as follows:
```bash
nmap --script-help="(ldap* or ssl*) and not (brute or broadcast or dos or external or fuzzer)"
```

#### Example Output
```bash
root@kali220:~# nmap -sV -p389 --script="(ldap* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" 10.10.10.119 
Starting Nmap 7.70 ( https://nmap.org ) at 2019-05-09 04:24 EDT 
NSE: [ssl-ccs-injection] No response from server: TIMEOUT 
Nmap scan report for target.com (10.10.10.119) 
Host is up (0.12s latency). 
PORT    STATE SERVICE VERSION 
389/tcp open  ldap    OpenLDAP 2.2.X - 2.3.X 
| ldap-rootdse: 
| LDAP Results 
|   <ROOT> 
|       namingContexts: dc=target,dc=com 
|       supportedControl: 2.16.840.1.113730.3.4.18 
|       supportedControl: 2.16.840.1.113730.3.4.2 
|       supportedControl: 1.3.6.1.4.1.4203.1.10.1 
|       supportedControl: 1.3.6.1.1.22 
|       supportedControl: 1.2.840.113556.1.4.319 
|       supportedControl: 1.2.826.0.1.3344810.2.3 
|       supportedControl: 1.3.6.1.1.13.2 
|       supportedControl: 1.3.6.1.1.13.1 
|       supportedControl: 1.3.6.1.1.12 
|       supportedExtension: 1.3.6.1.4.1.1466.20037 
|       supportedExtension: 1.3.6.1.4.1.4203.1.11.1 
|       supportedExtension: 1.3.6.1.4.1.4203.1.11.3 
|       supportedExtension: 1.3.6.1.1.8 
|       supportedLDAPVersion: 3 
|_      subschemaSubentry: cn=Subschema 
| ldap-search: 
|   Context: dc=target,dc=com 
|     dn: dc=target,dc=com 
|         objectClass: top 
|         objectClass: dcObject 
|         objectClass: organization 
|         o: target com 
|         dc: target 
|     dn: cn=Manager,dc=target,dc=com 
|         objectClass: organizationalRole 
|         cn: Manager 
|         description: Directory Manager 
|     dn: ou=People,dc=target,dc=com 
|         objectClass: organizationalUnit 
|         ou: People 
|     dn: ou=Group,dc=target,dc=com 
|         objectClass: organizationalUnit 
|         ou: Group 
|     dn: uid=user1,ou=People,dc=target,dc=com 
|         uid: user1 
|         cn: user1 
|         sn: user1 
|         mail: user1@target.com 
|         objectClass: person 
|         objectClass: organizationalPerson 
|         objectClass: inetOrgPerson 
|         objectClass: posixAccount 
|         objectClass: top 
|         objectClass: shadowAccount 
|         userPassword: {crypt}$x$xxxxxxxx$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 
|         shadowLastChange: 17691 
|         shadowMin: 0 
|         shadowMax: 99999 
|         shadowWarning: 7 
|         loginShell: /bin/bash 
|         uidNumber: 1000 
|         gidNumber: 1000 
|         homeDirectory: /home/user1 
|     dn: uid=user2,ou=People,dc=target,dc=com 
|         uid: user2 
|         cn: user2 
|         sn: user2 
|         mail: user2@target.com 
|         objectClass: person 
|         objectClass: organizationalPerson 
|         objectClass: inetOrgPerson 
|         objectClass: posixAccount 
|         objectClass: top 
|         objectClass: shadowAccount 
|         userPassword: {crypt}$x$xxxxxxxx$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 
|         shadowLastChange: 17691 
|         shadowMin: 0 
|         shadowMax: 99999 
|         shadowWarning: 7 
|         loginShell: /bin/bash 
|         uidNumber: 1001 
|         gidNumber: 1001 
|         homeDirectory: /home/user2 
|     dn: cn=user1,ou=Group,dc=target,dc=com 
|         objectClass: posixGroup 
|         objectClass: top 
|         cn: user1 
|         userPassword: {crypt}x 
|         gidNumber: 1000 
|     dn: cn=user2,ou=Group,dc=target,dc=com 
|         objectClass: posixGroup 
|         objectClass: top 
|         cn: user2 
|         userPassword: {crypt}x 
|_        gidNumber: 1001 
|_ssl-ccs-injection: No reply from server (TIMEOUT) 
| ssl-cert: Subject: commonName=target.com 
| Subject Alternative Name: DNS:target.com, DNS:localhost, DNS:localhost.localdomain 
| Issuer: commonName=target.com 
| Public Key type: rsa 
| Public Key bits: 1024 
| Signature Algorithm: sha256WithRSAEncryption 
| Not valid before: 2018-06-09T13:32:51 
| Not valid after:  2019-06-09T13:32:51 
| MD5:   0e61 1374 e591 83bd fd4a ee1a f448 547c 
|_SHA-1: 8e10 be17 d435 e99d 3f93 9f40 c5d9 433c 47dd 532f 
|_ssl-date: TLS randomness does not represent time 
| ssl-enum-ciphers: 
|   SSLv3: 
|     ciphers: 
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_IDEA_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D 
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_SEED_CBC_SHA (rsa 1024) - A 
|     compressors: 
|       NULL 
|     cipher preference: client 
|     warnings: 
|       64-bit block cipher 3DES vulnerable to SWEET32 attack 
|       64-bit block cipher IDEA vulnerable to SWEET32 attack 
|       Broken cipher RC4 is deprecated by RFC 7465 
|       CBC-mode cipher in SSLv3 (CVE-2014-3566) 
|       Ciphersuite uses MD5 for message integrity 
|   TLSv1.0: 
|     ciphers: 
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_IDEA_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D 
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_SEED_CBC_SHA (rsa 1024) - A 
|     compressors: 
|       NULL 
|     cipher preference: client 
|     warnings: 
|       64-bit block cipher 3DES vulnerable to SWEET32 attack 
|       64-bit block cipher IDEA vulnerable to SWEET32 attack 
|       Broken cipher RC4 is deprecated by RFC 7465 
|       Ciphersuite uses MD5 for message integrity 
|   TLSv1.1: 
|     ciphers: 
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_IDEA_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D 
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_SEED_CBC_SHA (rsa 1024) - A 
|     compressors: 
|       NULL 
|     cipher preference: client 
|     warnings: 
|       64-bit block cipher 3DES vulnerable to SWEET32 attack 
|       64-bit block cipher IDEA vulnerable to SWEET32 attack 
|       Broken cipher RC4 is deprecated by RFC 7465 
|       Ciphersuite uses MD5 for message integrity 
|   TLSv1.2: 
|     ciphers: 
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_AES_128_CBC_SHA256 (rsa 1024) - A 
|       TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_CBC_SHA256 (rsa 1024) - A 
|       TLS_RSA_WITH_AES_256_GCM_SHA384 (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_IDEA_CBC_SHA (rsa 1024) - A 
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D 
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D 
|       TLS_RSA_WITH_SEED_CBC_SHA (rsa 1024) - A 
|     compressors: 
|       NULL 
|     cipher preference: client 
|     warnings: 
|       64-bit block cipher 3DES vulnerable to SWEET32 attack 
|       64-bit block cipher IDEA vulnerable to SWEET32 attack 
|       Broken cipher RC4 is deprecated by RFC 7465 
|       Ciphersuite uses MD5 for message integrity 
|_  least strength: D 
| ssl-poodle: 
|   VULNERABLE: 
|   SSL POODLE information leak 
|     State: LIKELY VULNERABLE 
|     IDs:  CVE:CVE-2014-3566  OSVDB:113251 
|           The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other 
|           products, uses nondeterministic CBC padding, which makes it easier 
|           for man-in-the-middle attackers to obtain cleartext data via a 
|           padding-oracle attack, aka the "POODLE" issue. 
|     Disclosure date: 2014-10-14 
|     Check results: 
|       TLS_RSA_WITH_AES_128_CBC_SHA 
|       TLS_FALLBACK_SCSV properly implemented 
|     References: 
|       http://osvdb.org/113251 
|       https://www.imperialviolet.org/2014/10/14/poodle.html 
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566 
|_      https://www.openssl.org/~bodo/ssl-poodle.pdf 
|_sslv2-drown: 
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 30.57 seconds 
```

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
- [SANS - Understanding and Exploiting Web Based LDAP](https://pen-testing.sans.org/blog/2017/11/27/understanding-and-exploiting-web-based-ldap)
- [The LDAP Bind Operation](https://ldap.com/the-ldap-bind-operation/)
- [LDAPv3 - RFC 4510,4511,4512,4513,4514,4515,4516,4517,4518,4519](https://tools.ietf.org/html/rfc4510)
- [LDAP Injections - milw0rm](https://www.exploit-db.com/papers/13066)
- [LDAPman](http://www.ldapman.org/)
- [LDAP Injection - Examples of Vulnerable Code](https://www.immuniweb.com/vulnerability/ldap-injection.html)
- [(PDF) LDAP Injection & Blind LDAP Injection - Blackhat](https://www.blackhat.com/presentations/bh-europe-08/Alonso-Parada/Whitepaper/bh-eu-08-alonso-parada-WP.pdf)
- [(PDF) LDAP Injection - Sacha Faust](http://www.networkdls.com/articles/ldapinjection.pdf)
- [Testing for LDAP Injection - OWASP](https://www.owasp.org/index.php/Testing_for_LDAP_Injection_(OTG-INPVAL-006)
