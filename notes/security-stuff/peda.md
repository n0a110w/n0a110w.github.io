---
layout: default
title: How to Install Peda
parent: security stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}


## Install PEDA (Python Exploit Development Assistance for GDB
```bash
# download from git
git clone https://github.com/longld/peda.git ~/peda

# add to gdb
echo "source ~/peda/peda.py"" >> ~/.gdbinit

# run gdb
gdb -q <binary>
```

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }
[PEDA - github](https://github.com/longld/peda)


