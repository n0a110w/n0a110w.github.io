---
layout: default
title: yum 
parent: Package Management
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## Managing Packages

| command | description |
|:-------- |:------------ |
| `yum search <keyword>` | search for a package by keyword |
| `yum install <package>` | install package |
| `yum info <package>` | display info about package |
| `rpm -i package.rpm` | install package from local file |
| `yum remove <package>` | uninstall package |


## Managing Repos

| command | description |
|:-------- |:------------ |
| `yum repolist` or `yum repolist enabled` | show all enabled repositories |
| `yum repolist disabled` | show disabled repos |
| `yum repolist all` | show everything |

### Disable a repo
- edit the file in /etc/yum.repos.d/ and change enabled=1 to `enabled=0` or  
- disable using yum-config-manager: `yum-config-manager --disable <repo>`
  - <small>note: yum-config-manager is provided by `yum-utils`</small>


---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
#### Reference
{: .no_toc }


