---
layout: default
title: Docker reference
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## How to copy files from host to Docker Container
```bash
docker cp foo.tar.gz mycontainer:/foo.tar.gz
# mycontainer is a the container ID
# use `docker ps` to get the container ID
```




## Resources and More
{: .no_toc }
- <https://docs.docker.com/engine/reference/commandline/cp/>
