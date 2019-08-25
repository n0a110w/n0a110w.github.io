---
layout: default
title: awk
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### Divide a string every *N* characters
```
awk '{gsub(/.{2}/,"& ")}1' file
```

### How to print between two lines of specified strings
```
# print from string1 to string2 including the matched lines
awk '/string1/{f=1} /string2/{f=0;print} f'
awk '/string1/{f=1} f; /string2/{f=0}'
awk '/string1/,/string2/'

# print from string1 to string2 ignoring the matched lines
awk '/string1/{f=1;next} /string2/{f=0} f'
```




---

## Resources and More
{: .no_toc }
