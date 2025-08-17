---
layout: post
title: "Personal setup for ownCloud on Arch Linux, using Apache and MariaDB"
date: 2016-03-07 23:33:17
description: Official documentation describes installation on Red Hat/CentOS and Ubuntu but not openSUSE Leap. Here's a tutorial...
tags:
  - nextcloud
  - nextcloud hub
  - openSUSE
  - leap
  - cloud
categories:
  - NEXTCLOUD
  - openSUSE
  - English
twitter_text: "Personal setup for ownCloud on Arch Linux, using Apache and MariaDB"
---

{% highlight ruby %}
zypper in apache2 mariadb apache2-mod_php5 php5-gd php5-json php5-fpm php5-mysql php5-curl php5-intl php5-mcrypt php5-zip php5-mbstring php5-zlib
{% endhighlight %}

# Personal setup for ownCloud on Arch Linux, using Apache and MariaDB.

I used Banana Pi M1 for this setup but you can use it to Raspberry Pi or other Pi boards. I used Arch Linux as basic distro, Apache and MariaDB. Of cource there are Lighttpd, SQLite and other servers, but I would like to use those for bigger installations.

I assume you know how to install Arch Linux. I followed the [official wiki] (https://wiki.archlinux.org/index.php/OwnCloud).

This tutorial has the following sections:

- 1. Install needed extra software (including AUR)
- 2. Setup static ip
- 3. Setup no-ip or inadyn client
- 4. Setup MariaDB
- 5. Setup Apache
- 6. Extras
- 7. Setup ownCloud

---

### 1. Install needed extra software (including AUR)

First of all, setup the language.

{% highlight ruby %}
nano /etc/locale.gen
{% endhighlight %}
And uncomment en_US.UTF-8 (I uncomment el_GR.UTF-8 for Greek).

Then open the conf file.

{% highlight ruby %}
nano /etc/locale.conf
{% endhighlight %}

Copy and Paste the following.

{% highlight ruby %}
LANG="en_US.UTF-8"
LC_COLLATE="C"
LC_TIME="el_GR.UTF-8"
{% endhighlight %}

Now run the following.

{% highlight ruby %}
locale-gen
{% endhighlight %}

### Extra software

Install the programs that we'll use next.

{% highlight ruby %}
pacman -S apache mariadb owncloud php-apcu php-apache php-intl php-mcrypt exiv2 ntp mc make wget fakeroot sudo packer htop ntfs-3g pkg-config
{% endhighlight %}

#### AUR

After some changes on pacman, there are some workarounds on how to install AUR.

Add users to a group with the gpasswd command as root:

{% highlight ruby %}
gpasswd -a alarm wheel
{% endhighlight %}

Go to sudoers

{% highlight ruby %}
nano /etc/sudoers
{% endhighlight %}

and change the wheel line

{% highlight ruby %}

## Uncomment to allow members of group wheel to execute any command

%wheel ALL=(ALL) ALL
{% endhighlight %}

Reboot the system and give the follwing commands as user

{% highlight ruby %}
wget https://aur.archlinux.org/cgit/aur.git/snapshot/package-query.tar.gz
tar -xvzf package-query.tar.gz
cd package-query
makepkg -si
{% endhighlight %}

You can do the same with yaourt

{% highlight ruby %}
wget https://aur.archlinux.org/cgit/aur.git/snapshot/yaourt.tar.gz
tar -xvzf yaourt.tar.gz
cd yaourt
makepkg -si
{% endhighlight %}

### 2. Setup static ip

Edit the file eth0.network. Systemd-networkd uses \*.network files.

{% highlight ruby %}
nano /etc/systemd/network/eth0.network
{% endhighlight %}

paste the following (for static IP `192.168.1.100`)

{% highlight ruby %}
[Match]
Name=eth0

[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=8.8.8.8
DNS=8.8.4.4
{% endhighlight %}

You will then need to disable netcl. To find out what is enabled that is netctl related, run this:

{% highlight ruby %}
systemctl list-unit-files
{% endhighlight %}

Once you identify all netctl related stuff. Go through and disable all netctl related stuff. You may have more to disable than just the below:

{% highlight ruby %}
systemctl disable netctl@eth0.service
{% endhighlight %}

You will then need systemd-networkd enabled:

{% highlight ruby %}
systemctl enable systemd-networkd
{% endhighlight %}

Login with

{% highlight ruby %}
ssh alarm@192.168.1.100
{% endhighlight %}

### 3. Setup no-ip or inadyn client

If you installed AUR, here is how you can setup no-ip service.

Install No-IP Client with yaourt.

{% highlight ruby %}
yaourt -S noip
{% endhighlight %}

_Configure No-IP Back-end_

Create the configuration file.

{% highlight ruby %}
noip2 -C
{% endhighlight %}

Enter the relevant details when prompted. All settings can be modified individually later.

Modify the update interval.

{% highlight ruby %}
noip2 -U 30
{% endhighlight %}

Interval that the IP will be updated is set with -U option in minutes. Default is 30 minutes.

Modify No-IP username.

{% highlight ruby %}
noip2 -u username
{% endhighlight %}

Modify No-IP password.

{% highlight ruby %}
noip2 -p password
{% endhighlight %}

Start noip service.

{% highlight ruby %}
systemctl start noip2
{% endhighlight %}

Enable noip serive.

{% highlight ruby %}
systemctl enable noip2
{% endhighlight %}

### 4. Setup MariaDB

After MariaDB is installed, setup the database.

{% highlight ruby %}
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
{% endhighlight %}
Then change default password.

{% highlight ruby %}
mysqld_safe --skip-grant-tables &
{% endhighlight %}

{% highlight ruby %}
mysql_secure_installation
{% endhighlight %}

Create database for ownCloud.
{% highlight ruby %}
mysql -u root -p

CREATE DATABASE owncloudb;

GRANT ALL ON owncloudb.\* TO ocuser@localhost IDENTIFIED BY 'dbpass';
{% endhighlight %}

exit the environement.

You added the following (change those as you wish):

{% highlight ruby %}
DATABASE: owncloudb
USER: ocuser
PASSWORD: dbpass
HOST: localhost
{% endhighlight %}

Start the service and enable it.

{% highlight ruby %}
systemctl start mysqld.service
systemctl enable mysqld.service
{% endhighlight %}

### 5. Setup Apache

Check the [ownCloud Arch wiki] (https://wiki.archlinux.org/index.php/OwnCloud#Apache_configuration)

First of all copy the owncloud.conf file.

{% highlight ruby %}
cp /etc/webapps/owncloud/apache.example.conf /etc/httpd/conf/extra/owncloud.conf
{% endhighlight %}

Change the open_basedir parameter

{% highlight ruby %}
nano /etc/httpd/conf/extra/owncloud.conf
{% endhighlight %}

and add change the following

{% highlight ruby %}
php_admin_value open_basedir "/srv/http/:/dev/urandom:/tmp/:/usr/share/pear/:/usr/share/webapps/owncloud/:/etc/webapps/owncloud:/mnt/owncloud_data/"
{% endhighlight %}

Then open httpd.conf.

{% highlight ruby %}
nano /etc/httpd/conf/httpd.conf
{% endhighlight %}

And add at the end
{% highlight ruby %}
Include conf/extra/owncloud.conf
{% endhighlight %}

also comment the line

{% highlight ruby %}
#LoadModule mpm_event_module modules/mod_mpm_event.so
{% endhighlight %}

and uncomment the line:

{% highlight ruby %}
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
{% endhighlight %}

To enable PHP, add these lines

Place this in the LoadModule list anywhere after LoadModule dir_module modules/mod_dir.so:

{% highlight ruby %}
LoadModule php7_module modules/libphp7.so
{% endhighlight %}

Place this at the end of the Include list:

{% highlight ruby %}
Include conf/extra/php7_module.conf
{% endhighlight %}

Now open the php.ini
{% highlight ruby %}
nano /etc/php/php.ini
{% endhighlight %}

first find the open_basedir and add the line

{% highlight ruby %}
open_basedir = /srv/http/:/dev/urandom:/usr/share/webapps/:/var/www/owncloud/apps/:/mnt/owncloud_data/
{% endhighlight %}

and then enable the extensions

{% highlight ruby %}
gd.so
iconv.so
xmlrpc.so
zip.so
bz2.so
curl.so
intl.so
mcrypt.so
pdo_mysql.so
exif.so
{% endhighlight %}

Start and enable the serivce.

{% highlight ruby %}
systemctl start httpd.service
systemctl enable httpd.service
{% endhighlight %}

### 6. Extras

- Start Network Time Protocol daemon.

{% highlight ruby %}
systemctl start ntpd.service
systemctl enable ntpd.service
{% endhighlight %}

- Enable [Caching] (https://wiki.archlinux.org/index.php/OwnCloud#Caching)

Make sure you have the package php-apcu installed. Open the file apcu.ini.

{% highlight ruby %}
nano /etc/php/conf.d/apcu.ini
{% endhighlight %}

and enable the extension

{% highlight ruby %}
extension=apcu.so
{% endhighlight %}

Then open php.ini.

{% highlight ruby %}
nano /etc/php/php.ini
{% endhighlight %}

And add

{% highlight ruby %}
[apc]
apc.enable_cli=1
{% endhighlight %}

Finally, open the file config.php.

{% highlight ruby %}
nano /etc/webapps/owncloud/config/config.php
{% endhighlight %}

{% highlight ruby %}
'memcache.local' => '\OC\Memcache\APCu',
{% endhighlight %}

- [Permissions](https://wiki.archlinux.org/index.php/OwnCloud#Setting_strong_permissions)

For hardened security we recommend setting the permissions on your ownCloud directories as strictly as possible, and for proper server operations. This should be done immediately after the initial installation and before running the setup. Your HTTP user must own the config/, data/ and apps/ directories so that you can configure ownCloud, create, modify and delete your data files, and install apps via the ownCloud Web interface.

Create new file:

{% highlight ruby %}
nano oc-perms
{% endhighlight %}

and add the following

{% highlight ruby %}
#!/bin/bash
ocpath='/usr/share/webapps/owncloud'
htuser='http'
htgroup='http'
rootuser='root'

printf "Creating possible missing Directories\n"
mkdir -p $ocpath/data
mkdir -p $ocpath/assets

printf "chmod Files and Directories\n"
find ${ocpath}/ -type f -print0 | xargs -0 chmod 0640
find ${ocpath}/ -type d -print0 | xargs -0 chmod 0750

printf "chown Directories\n"
chown -R ${rootuser}:${htgroup} ${ocpath}/
chown -R ${htuser}:${htgroup} ${ocpath}/apps/
chown -R ${htuser}:${htgroup} ${ocpath}/config/
chown -R ${htuser}:${htgroup} ${ocpath}/data/
chown -R ${htuser}:${htgroup} ${ocpath}/themes/
chown -R ${htuser}:${htgroup} ${ocpath}/assets/

chmod +x ${ocpath}/occ

printf "chmod/chown .htaccess\n"
if [ -f ${ocpath}/.htaccess ]
then
chmod 0644 ${ocpath}/.htaccess
  chown ${rootuser}:${htgroup} ${ocpath}/.htaccess
fi
if [ -f ${ocpath}/data/.htaccess ]
 then
  chmod 0644 ${ocpath}/data/.htaccess
  chown ${rootuser}:${htgroup} ${ocpath}/data/.htaccess
fi
{% endhighlight %}

make the file executable

{% highlight ruby %}
chmod +x oc-perms
{% endhighlight %}

### 7. Setup ownCloud

Create data directory (owncloud_data) and give the right permissions.

{% highlight ruby %}
mkdir /mnt/owncloud_data
chmod -R 0770 /mnt/owncloud_data
chown -R http:http /mnt/owncloud_data
{% endhighlight %}

Open the static IP 192.168.1.100 and add the username and password for root. Add data folder (/mnt/owncloud_data) and username-password of the MariaDB database.

If you want to access your ownCloud when you're away from home, first you should open the port 80. Add port forward to your owncloud instance. Then you should add the no-ip domain to the file

{% highlight ruby %}
nano /etc/webapps/owncloud/config/config.php
{% endhighlight %}

add the following

{% highlight ruby %}
1 => 'domain.no-ip.org'
{% endhighlight %}
