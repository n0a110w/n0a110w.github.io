---
layout: default
title: awk
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## One-liners:
### Divide a string every *N* characters
```awk
awk '{gsub(/.{2}/,"& ")}1' file
```



---

## Resources and More
{: .no_toc }
