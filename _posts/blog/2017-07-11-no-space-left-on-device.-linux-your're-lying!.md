---
layout: post
categories: [linux sucks]
tags: [No space left on device, manjaro, linux]
permalink: /:categories/:year/:month/:title:output_ext
hero-img:
featured-img:
---

One would think that if you have plenty of free disk space, you should be able to install an application, and then use it, but that would be to easy. Rather, its not really a problem of disk space, but running out of temporary space during install.

## System
- Default install of Manjaro XFCE Edition (17.0.1) with the install option: install along side.
- 4 GB system memory.

## The Problem
This error occurred while installing a large application `No space left on device.`

Part of the problem is /tmp fills up. After an install attempt fails, the failed content is left in /tmp and RAM, leaving the system crippled till reboot.
Disk file system usage can be seen with the command:

```bash
df
```

The other problem, at least in my case, was that there was no swap partition, this occurred when installing Manjaro as the second OS in a dual boot setup.
A swap partition can be setup manually during install or after.
swap and memory sizes can be seen with the command:

```bash
free
```

## The Fix
- Temporarily resize the tmpfs /tmp

```bash
mount -o remount,size=8G,noatime /tmp
```

A swap partition or swap file can be added post install easily enough.

[manjaro wiki](https://wiki.manjaro.org/index.php?title=Add_a_/swapfile)

```bash
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```
- open fstab

```bash
sudo nano /etc/fstab
```

- Add this line

```bash
/swapfile none swap defaults 0 0
```

No more "No space left on device" issue
