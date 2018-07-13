---
layout: post
categories: linux-sucks
tags: USB-To-Serial Hercules-terminal playOnLinux wine
permalink: /:categories/:year/:month/:title:output_ext
---

There's a serial terminal I like (hardware group's Hercules terminal), which as far as I know is only available for Windows. PlayOnLinux makes wine easy to use. Wine makes it possibly possible to run windows applications. Having setup the environment, the serial terminal application opens, but it cant open a com port, permission denied.

When using serial terminal application, whether its run through an wine or not, the non-root user needs permissions. First thing is to add user to dialout group. Why dialout group? Maybe some archaic name used when serial modems were the it thing, and would dialout over an analog phone line.

```console
sudo adduser $USER dialout
```

Now the application in wine needs to be able to find the USB to serial port. All the Com port options in the HWG Hercules are Com#, there isn't a ttyUSB option, so a symbolic link is needed here.

```console
ln -s /dev/ttyUSB0 ~/PlayOnLinux\'s\ virtual\ drives/'win_env'/dosdevices/com3
```

Where 'win_env' is the name of the PlayOnLinux windows environment name. This line can be added to PlayOnLinux configuration. Open the PlayOnLinux configuration, select the application, open Miscellaneous tab and add the above line with the appropriate PlayOnLinux environment path.

<img src="/images/blog/playonlinux/ln-config.jpg" class="img-fluid"/>
