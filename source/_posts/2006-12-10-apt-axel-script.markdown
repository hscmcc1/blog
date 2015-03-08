---
author: hesicong
comments: true
date: 2006-12-10 22:12:46+00:00
layout: post
slug: apt-axel-script
title: apt-axel 脚本
wordpress_id: 102
categories:
- 其他
tags:
- ubuntu
---

我把apt-axel脚本更改了一下，以适合自己的使用：
修改如下：
* 使用一个server
* 使用mirror server达到更快的速度

下面是apt-axel文件

```
#!/bin/bash

###########################################################################
#
# Authors: Jes鷖 Espino Garc韆 & Lucas Garc韆
# Email: jespino@imap.cc
# Date: 31/05/2004
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#
###########################################################################

# If the config file not exist, load the default parameters

VERSION="0.1"
CONFIG_FILE="/etc/apt-axel.conf";

if [ -f $CONFIG_FILE ]; then
. $CONFIG_FILE
else
# Verbose TRUE or FALSE
VERBOSE="FALSE"

# Conections
CONNECTIONS=8

# Servers
server1="ftp://ftp.de.debian.org/debian/"
server2="ftp://ftp.es.debian.org/debian/"
server3="ftp://ftp.fr.debian.org/debian/"
server4="ftp://ftp.it.debian.org/debian/"
server5="ftp://ftp.fi.debian.org/debian/"
server6="ftp://ftp.uk.debian.org/debian/"
server7="ftp://ftp.tk.debian.org/debian/"
server8="ftp://ftp.us.debian.org/debian/"
fi

# Set the package variable to be global
package="";

# Show the help
mostrar_ayuda() {
echo "Usage: apt-axel  ";
echo "";
echo "Options:";
echo "  install - Install new packages (paquete es libc6 y no libc6.deb)";
echo "  upgrade - Do a software upgrade";
echo "  dist-upgrade - Do a distribution upgrade, see apt-get(8)";
echo "  --version - Print the current version of apt-axel";
echo "  --help - Show this help";
echo "";
}

# Get a package from the ftp server
get_package() {
descargar() {
# Check the $VERBOSE variable and the filesize, if the filesize is lower than 200K will use only 4 conections
# And if the $VERBOSE variable is TRUE, then print the axel output
if [ $VERBOSE == "TRUE" ]; then
if [ $size -gt 200000 ]; then
echo $server1$url
axel -a -n $CONNECTIONS $server1$url -o /tmp/$archivo -S $mirror1
else
axel -a -n 4 $server1$url -o /tmp/$archivo -S $mirror1
fi
else
echo -n "Geting $package package: "
if [ $size -gt 200000 ]; then
axel -a -n $CONNECTIONS $server1$url -o /tmp/$archivo -S $mirror1 > /dev/null
else
axel -a -n 4 $server1$url -o /tmp/$archivo -S $mirror1 > /dev/null
fi
echo "Done"
fi
}

# Getting data
url=$(apt-cache show $package | grep ^Filename: | sed s/^Filename: //)
archivo=$(echo "$url" | sed s/^.*$package/$package/)
pkgstatus=$(dpkg -s $package 2> /dev/null | grep ^Status: | grep -v "not-installed")
md5sum=$(apt-cache show $package | grep ^MD5sum: | sed s/^MD5sum: //)
size=$(apt-cache show $package | grep ^Size: | sed s/^Size: //)

# Downloading the package
if [ -f /var/cache/apt/archives/$archivo ]; then
echo -n "Package already downloaded. Checking md5sum of $package package: "
while [ $md5sum != $(md5sum /var/cache/apt/archives/$archivo | sed s/ .*$//) ]; do
echo "incorrect"
echo "Downloading again $package package."
rm -f /var/cache/apt/archives/$archivo
descargar
done
echo "correct"
else
descargar
fi

# Move the file to /var/cache/apt/archives
if [ -f /tmp/$archivo ]; then
if [ $md5sum == $(md5sum /tmp/$archivo | sed s/ .*$//) ]; then
mv -f /tmp/$archivo /var/cache/apt/archives/
fi
fi
}

pkg_install() {
for package in $(apt-get -s install $1 | grep ^Inst | sed s/^Inst // | sed s/ .*$//); do
get_package
done
apt-get -y install $1
}

upgrade() {
for package in $(apt-get -s upgrade | grep ^Inst | sed s/^Inst // | sed s/ .*$//); do
get_package
done
apt-get -y upgrade
}

dist_upgrade() {
for package in $(apt-get -s dist-upgrade | grep ^Inst | sed s/^Inst // | sed s/ .*$//); do
get_package
done
apt-get -y dist-upgrade
}

#
# Main program
#
if [ `id -u` != 0 ]; then
echo "You must be root to run this command."
else
case $1 in
install) pkg_install $2;;
upgrade) upgrade;;
dist-upgrade) dist_upgrade;;
(--version) echo "Current Version: $VERSION";;
(--help | -h) mostrar_ayuda;;
*) mostrar_ayuda;;
esac
fi
```

配置文件：
GNU nano 1.3.12           File: /etc/apt-axel.conf

```
# Verbose TRUE or FALSE
VERBOSE="TRUE"

# Conections
CONNECTIONS=20

# Servers
server1="http://ubuntu.cnsite.org/ubuntu/"
mirror1="http://ubuntu.cn99.com/ubuntu/"
```
[download id="24"]
