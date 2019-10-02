---
layout: default
title: nextcloud
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

### Install Dependencies (Debian Buster)
```bash
# install core packages
apt-get install apache2 mariadb-server libapache2-mod-php
# install php modules
apt-get install apt-get install php-gd php-json php-mysql php-curl php-mbstring php-intl php-imagick php-xml php-zip php-cli php-cgi
```
### Setup database
```bash
# run the secure installation script to setup the root db account
sudo mysql_secure_installation

# sign into MariaDB using the root password
sudo mysql -u root -p

# create a new db
CREATE DATABASE nextcloud;

# create new user for the the nextcloud db
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'yourpassword';

# grant full privileges on the db for the new user
GRANT ALL ON nextcloud.* TO 'nextclouduser'@'localhost';

# flush privileges and quit
FLUSH PRIVILEGES;
\q
```
### Download and Install
```bash
# download Nextcloud archive
wget https://download.nextcloud.com/server/releases/nextcloud-17.0.0.tar.bz2

wget https://download.nextcloud.com/server/releases/nextcloud-17.0.0.tar.bz2.md5
md5sum -c nextcloud-x.y.z.tar.bz2.md5 < nextcloud-x.y.z.tar.bz2

# extract the archive
tar -xjf nextcloud-x.y.z.tar.bz2

# copy the nextcloud directory to the http server root
cp -r nextcloud /var/www/html/

# give ownership of the nextcloud directory to www-data
sudo chown -R www-data:www-data /var/www/html/nextcloud

# that's it!
# nextcloud dashboard should be accessible at localhost/nextcloud
# if you need access to the UI from somewhere other than localhost, change the trusted domains section of config/config.php
```

## How to setup SSL cert (Let's Encrypt)
```bash
# tested under Debian Buster
# add repo
sudo add-apt-repository ppa:certbot/certbot
# install certbot
sudo apt install python-certbot-apache
# start interactive session to get certificate
sudo certbot --apache -d <yourdomain>
```


[letsencrypt-certificate-dns-verification-noip](https://hometechhacker.com/letsencrypt-certificate-dns-verification-noip/)
[nextcloud official installation docs](https://docs.nextcloud.com/server/17/admin_manual/installation/source_installation.html)
