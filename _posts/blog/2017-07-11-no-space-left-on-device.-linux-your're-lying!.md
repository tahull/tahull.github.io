---
layout: post
categories: linux-sucks
tags: No-space-left-on-device manjaro
permalink: /:categories/:year/:month/:title:output_ext
hero-img:
featured-img:
---

Well sort of. One would think that if you have 100gb+ of disk space free, you should be able to install an application, and then use it, but that would be to easy.

## System
default install of Manjaro XFCE Edition (17.0.1) with the install option: install along side (as in dual boot) - this is important later

This error occurred while installing a large application `No space left on device.`

part of the problem is /tmp fills up. After an install attempt fails, the failed content is left in /tmp and RAM, leaving the system crippled till reboot.
Disk file systems usage can be seen with the command:

```
df
```

Solution: temporarily resize the tmpfs /tmp

```
mount -o remount,size=8G,noatime /tmp
```

The other problem, at least in my case, was that there was no swap partition, this occurred when installing Manjaro as the second OS in a dual boot setup.
If I was paying attention I might of setup manual partitions instead of accepting the default.
swap and memory sizes can be seen with the command:

```
free
```

A swap partition or swap file can be added post install easily enough.

[manjaro wiki](https://wiki.manjaro.org/index.php?title=Add_a_/swapfile)

```
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```
open fstab
```
sudo nano /etc/fstab
```

and add this line

```
/swapfile none swap defaults 0 0
```


success, no more "No space left on device" issue
