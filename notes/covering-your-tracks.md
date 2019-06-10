---
layout: default
title: Covering Your Tracks
nav_order: 2
---

```bash
# clear auth.log file
ln /dev/null ~/.bash_history -sf

# clear current user .bash_history file
echo "" > ~/.bash_history -rf

# delete ~/.bash_history file
rm ~/.bash_history -rf

# clear current session history
history -c

# set history max lines to 0
export HISTFILESIZE=0

# set history max commands to 0
export HISTSIZE=0

# disable history logging (need to logout to take effect) 
unset HISTFILE

# kills current session
kill -9 $$
```
