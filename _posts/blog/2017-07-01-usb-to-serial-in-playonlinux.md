---
layout: post
categories: [setup guide]
tags: [linux, USB To Serial, Hercules terminal, playOnLinux, wine, ubuntu 16.04]
permalink: /blog/:year/:month/:title
redirect_from:
  - /linux%20sucks/2017/07/USB-to-serial-in-PlayOnLinux.html
featured-img: /images/features/shell-feature.png
---

There's a serial terminal I like (hardware group's Hercules terminal), which as far as I know is only available for Windows. Play On Linux makes wine easy to use. Wine makes it possibly possible to run windows applications. Play On Linux can be installed through the app store or command line easy enough.

```shell
sudo apt install playonlinux
```

Open the Play On Linux application. The GUI is easy to use and has lots of preconfigured options for installing built-for-windows applications/games. The Hercules terminal isn't in that list, but there's an option to `install a non-listed program` and the GUI guides you through the creating a new Wineprefix or using and existing Wineprefix. Hercules terminal is a stand alone exe, just move the exe to the new Wineprefix.

After the Hercules terminal is installed, the application opens, but it cant open a com port, permission denied.

When using serial terminal application, whether its run through Wine or not, the non-root user needs permissions. First thing is to add user to dialout group. Why dialout group? Maybe some legacy name used when serial modems were popular, and would dial out over a phone line.

Check if you're already in a group with
```shell
groups
```
If `dialout` isn't in that list, then add it with
```shell
sudo adduser $USER dialout
```
After adding a new group to the user, logout and login again.

Now the application needs to be able to find the USB to serial port. All the Com port options in the HWG Hercules are Com#, there isn't a ttyUSB option, so a symbolic link is needed here.

```shell
ln -s /dev/ttyUSB0 ~/PlayOnLinux\'s\ virtual\ drives/'win_env'/dosdevices/com3
```

Where 'win_env' is the name of the Play On Linux windows environment name. This line can be added to Play On Linux configuration. Open the Play On Linux configuration, select the application, open Miscellaneous tab and add the above line with the appropriate Play On Linux environment path.

<img src="/images/blog/playonlinux/ln-config.jpg" alt="image of PlayOnLinux configuration window" title = "PlayOnLinux configuration" class="img-fluid"/>

## Update
A native alternative to Hercules terminal is moserial. It is a serial terminal only, if I need a tcp or udp terminal I still use Hercules terminal.

```shell
sudo apt install moserial
```
