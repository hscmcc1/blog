---
author: hesicong
comments: true
date: 2009-08-21 11:11:17+00:00
layout: post
slug: gnome-iso-mount-and-uninstall-script
title: Gnome ISO挂载和卸载的脚本
wordpress_id: 1957
categories:
- 软件
tags:
- gnome
- iso
- ubuntu
---

gedit $HOME/.gnome2/nautilus-scripts/mount_iso

``` bash
#!/bin/bash
#

gksudo -u root -k /bin/echo "got root?"

sudo mkdir /media/"$*"

if sudo mount -o loop "$*" /media/"$*"
then
if zenity --question --title "ISO Mounter" --text "$* Successfully Mounted."

then
nautilus /media/"$*" --no-desktop
fi
exit 0
else
sudo rmdir /media/"$*"
zenity --error --title "ISO Mounter" --text "Cannot mount $*!"
exit 1
fi
```

gedit $HOME/.gnome2/nautilus-scripts/umount_iso

``` bash
#!/bin/bash
#

for I in "$*"
do
foo=`gksudo -u root -k -m "enter your password for root terminal
access" /bin/echo "got r00t?"`

sudo umount "$I" && zenity --info --text "Successfully unmounted /media/$I/" && sudo rmdir "/media/$I/"
done
done
exit 0
```

```
sudo chmod 755 ~/.gnome2/nautilus-scripts/*_iso
```

然后重启x: `ctrl+alt+backspace`
