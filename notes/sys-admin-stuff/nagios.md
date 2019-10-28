---
layout: default
title: nagios
parent: sys admin stuff
nav_order: 2
---

1. TOC
{:toc}

## How to install Nagios Core (Ubuntu 18.10)
```bash
# install dependencies
sudo apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.2 libgd-dev

# download the source
cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.1.tar.gz
tar xzf nagioscore.tar.gz

# compile
cd /tmp/nagioscore-nagios-4.4.1
sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
sudo make all

# create user and group
sudo make install-groups-users
sudo usermod -aG nagios www-data

# install binaries
sudo make install

# install daemon
sudo make install-daemoninit

# install command mode
sudo make install-commandmode

# install *sample* configuration files
sudo make install-config

# Install Apache config files
sudo make install-webconf
sudo a2enmod rewrite
sudo a2enmod cgi

# configure firewall ; allow port 80 inbound traffic for accessing the web interface
sudo ufw allow Apache
sudo ufw reload

# create apache user account: nagiosadmin
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
# When adding additional users in the future, you need to remove -c from the above command otherwise it will replace the existing nagiosadmin user (and any other users you may have added)

# # restart apache and point your browser to the FQDN or IP ADDR of the server: ( ie. http://ipaddress/nagios )
systemctl restart apache2.service
```

---

## How to install Nagios Core (Debian Buster)
```bash
# install dependencies
apt-get update -y
apt-get install -y autoconf gcc libc6 make wget unzip apache2 apache2-utils php libgd-dev libuser

# download the source
cd /tmp
wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.5.tar.gz
tar xzf nagioscore.tar.gz

# compile
cd /tmp/nagioscore-nagios-4.4.5/
./configure --with-httpd-conf=/etc/apache2/sites-enabled
make all

# create user and group
sudo make install-groups-users
sudo usermod -aG nagios www-data

# install binaries
sudo make install

# install daemon
sudo make install-daemoninit

# install command mode
sudo make install-commandmode

# install *sample* configuration files
sudo make install-config

# Install Apache config files
sudo make install-webconf
sudo a2enmod rewrite
sudo a2enmod cgi

# configure firewall ; allow port 80 inbound traffic for accessing the web interface
iptables -I INPUT -p tcp --destination-port 80 -j ACCEPT
apt-get install -y iptables-persistent

# create apache user account: nagiosadmin
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
# When adding additional users in the future, you need to remove -c from the above command otherwise it will replace the existing nagiosadmin user (and any other users you may have added)

# restart apache and point your browser to the FQDN or IP ADDR of the server: ( ie. http://ipaddress/nagios )
systemctl restart apache2.service
```

---

## How to install Nagios Plugins (v2.2.1)
```bash
# dependencies
apt-get install -y autoconf gcc libc6 libmcrypt-dev make libssl-dev wget bc gawk dc build-essential snmp libnet-snmp-perl gettext

# download the source
cd /tmp
wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.2.1.tar.gz
tar zxf nagios-plugins.tar.gz

# compile and install
cd /tmp/nagios-plugins-release-2.2.1/
sudo ./tools/setup
sudo ./configure
sudo make
sudo make install

# restart the service
sudo systemctl restart nagios.service
```

---

## How to verify Nagios configuration
```bash
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

---

## Resources and More
{: .no_toc }
