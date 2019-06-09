---
layout: default
title: LAMP
parent: sys admin stuff
#has_children: true
nav_order: 2
---

1. TOC
{:toc}

## How to Install LAMP on CentOS 7:
1. Install / Start Apache
```bash
# install 
yum install httpd
# start apache
systemctl start httpd.service
# enable apache to start on boot
systemctl enable httpd.service
```
2. Install MariaDB
```bash
# install
yum install mariadb-server mariadb
# start mariadb
systemctl start mariadb
# run simple security installation script
mysql_secure_installation
# enable mariadb on boot
systemctl enable mariadb.service
```
3. Install PHP
```bash
# install
yum install php php-mysql
# spot-check the phpinfo() function
echo "<?php phpinfo(); ?>" >> /var/www/html/info.php
# restart webserver
systemctl restart httpd.service
# verify the page loads the phpinfo() function at http://server/info.php
```
  - NOTE: The official CENTOS 7 repos have PHP 5.4 which is EOL. To install the latest php version, perform the following
```bash
# enable EPEL and Remi repositories
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
# install yum-utils
yum install yum-utils
# enable remi repo for installing different php versions
yum-config-manager --enable remi-php72
yum-config-manager --enable remi-php71
yum-config-manager --enable remi-php56
# Now install php with all necessary modules
yum install php php-common php-cli php-mysql php-xml php-xcache php-gd php-mbstring
# double check new version
php -v
```
4. Configure Firewall
```bash
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

### How to create MySQL USER and DB via CLI
1. Connect to the MySQL engine:
```bash
mysql -u root -p
```
2. Create DATABASE
```bash
CREATE DATABASE new_database_name
```
3. Create user and grant privileges 
```bash
GRANT ALL PRIVILEGES ON new_database_name.* TO "newuser"@"localhost" IDENTIFIED BY "password123"
FLUSH PRIVILEGES
```

### How to Install VSFTPD, FTP
1. Install VSFTPD and FTP
```bash
yum install vsftpd ftpd
```
2. Modify the vsftpd.conf
```bash
vi /etc/vsftpd/vsftpd.conf
```
  - Disable anonymous login
  - uncomment `ascii_upload_enable=YES` and `ascii_download_enable=YES`
3. Start the vsftpd service
```bash
systemctl start vsftpd
systemctl enable vsftpd
```
4. Configure firewall 
firewall-cmd --permanent --add-port=21/tcp
firewall-cmd --permanent --add-service=ftp
firewall-cmd --reload

5. Update the SELinux boolean values for FTP service
```bash
setsebool -P ftp_home_dir on
```
6. Create FTP user
```bash
useradd webftp
passwd webftp 
chown -R webftp /var/www/html
```

---

## Resources and More
### Links
#### Reference


