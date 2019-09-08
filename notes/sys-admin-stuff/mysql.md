---
layout: default
title: mysql
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## Reset MySQL Password without root password
```bash
# stop the mysql service
service mysql stop

# start the mysqld_safe service
mysqld_safe --skip-grant-tables --skip-networking &

# now connect to the mysql server as root, with NO password
mysql -u root

# now reset the password
mysql> use mysql;
mysql> update user set authentication_string=password('QWERTY123') where user='root';
mysql> flush privileges;
mysql> quit
# password changed
# restart the mysql service
# done!
```




## Resources and More
{: .no_toc }
