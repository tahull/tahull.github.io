---
layout: post
categories: hacks
tags: [openocd, hacks, parport, jtag wiggler]
permalink: /:categories/:year/:month/:title:output_ext
hero-img:
featured-img: /images/blog/openocd/parport-feature.jpg
---

Parallel ports aren't a popular port and so, don't have much support. A Windows driver giveio.sys was previously used on 32-bit Windows to support parallel port jtag device use with OpenOCD. There isn't a 64-bit version of giveio.sys. OpenOCD can't communicate with parallel port on a 64-bit windows OS using giveio.sys and results with an error: `Error: missing privileges for direct i/o`.

There are some alternative 64-bit parallel port drivers that can be used with 64-bit windows OS. The driver I went with is InpOutx64 found here: [inpout](http://www.highrez.co.uk/downloads/inpout32/). There's a couple steps to make this work
- Installing the driver and including the inpoutx64.dll in openocd/bin folder
- Modify OpenOCD code to use inpoutx64 instead of giveio

<img src="/images/blog/openocd/parport-fail.png" alt="image of openOCD parport missing privileges" title = "openOCD missing privileges" class="img-fluid"/>

OpenOCD could be modified for 64-bit windows; this guide will show the steps I took to get my parallel port JTAG wiggler to work with 64-bit Windows 10.

## The Steps
### 64-bit parallel port driver
download [inpout32](http://www.highrez.co.uk/downloads/inpout32/)
Extract and run the driver installer aptly named `InstallDriver.exe` in the `InpOutBinaries_1501\Win32` sub folder

### Modify the OpenOCD source
I can't take much credit here as I found that someone had already suggested a patch to use inpoutx64. Original post here and patch: [message board](https://sourceforge.net/p/openocd/mailman/message/28350777/), for whatever reason, that patch wasn't applied to the OpenOCD project.

So, here are the source files of my applied modifications: [OpenOCDModFiles](https://github.com/tahull/OpenOCDModFiles). The specific source file for enabling parallel port functionality is `src/jtag/driver/parport.c`

### Building
Follow the build procedure at [gnuarmoeclipse](http://gnuarmeclipse.github.io/openocd/build-procedure/)
copy/replace the `parport.c` before building with `/build-openocd.sh --Win64`

### Setup inpoutx64.dll
Go back to the extracted `InpOutBinaries_1501` folder and copy the `inpoutx64.dll` and place it in the `openocd/bin` folder (same folder as the openocd.exe)

## Testing OpenOCD
open a cmd shell, cd to the location of the OpenOCD.exe and run
`openocd -f interface/parport.cfg -f target/"target processor"` in  this case `-f target/lpc2138.cfg`

Now OpenOCD should be running

<img src="/images/blog/openocd/parport-success.png" alt="image of openOCD connected through parport" title = "openOCD connected through parport" class="img-fluid"/>
