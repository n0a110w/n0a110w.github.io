---
layout: default
title: misc stuff
parent: command reference
grand_parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### List the binaries installed by a Debian package:
```bash
binaries () { for f in $(dpkg -L "$1" | grep "/bin/"); do basename "$f"; done; }

# i prefer this as a fish function:
$ cat ~/.config/fish/functions/binaries.fish
function binaries
        for f in (dpkg -L $argv | grep "/bin/")
                basename $f
        end
end
```


---

## Resources and More
{: .no_toc }
