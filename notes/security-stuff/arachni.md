---
layout: default
title: arachni
parent: security stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}

Arachni is a tool used to audit web applications. 
## Installing Arachni from source (github)
<small>
```bash
# 
git clone https://github.com/Arachni/arachni.git
cd arachni
gem install bundler # may need to use sudo
bundle install --without prof # resolve possible dev dependencies
rake install # optional, will install to PATH
```

## Running Arachni (cli)
```bash
# scan the target for all types of vulnerabilities
./arachni <target>
# note: stop the scan by pressing <enter>

# scan the target for specific vulnerability
# in this example, the sql_injection_timing check is selected
./arachni <target> --checks= sql_injection_timing

# you can also apply the wildcard to select a variety of a checks
./arachni <target> --checks= sql*

# it is also possible to exclude checks from the scanning process
# in this example, all xss vulnerability tests are excluded
./arachni <target> --checks= *, -xss*
```
- Note: A complete list of command line options can be found on the project's [wiki](https://github.com/Arachni/arachni/wiki/Command-line-user-interface)

## Running Arachni (web)
```bash
# starting arachni web
./arachni_web
./arachni_rpcd

# access the WebUI at localhost:9292
# default administrator email , password
# admin@admin.admin , administrator

# default user email , password
# user@user.user , regular_user
```
- Note: More info on the Arachni WebUI can be found [here](https://github.com/Arachni/arachni-ui-web/wiki)

---

## Resources and More
{: .no_toc }
### Links
{: .no_toc }
[arachni - github](https://github.com/Arachni/arachni)
#### Reference
{: .no_toc }


