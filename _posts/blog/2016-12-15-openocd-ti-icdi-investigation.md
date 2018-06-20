---
layout: post
categories: openocd
tags: openocd ti icdi jtag windows
permalink: /:categories/:year/:month/:title:output_ext
hero-img:
featured-img: /images/blog/openocd/ti-icdi-feature.jpg
---

Ideally OpenOCD should work with ICDI out of the box, in my case it didn't. OpenOCD uses libusb and the TI ICDI driver uses winusb. libusb can talk to winusb, so there should be no need to use the Zadig tool to modify the ICDI jtag/swd interface. However, OpenOCD fails to open the interface and results with the error: `Error: open failed`

The Texas Instruments ICDI, comes as a Composite USB device with a virtual com port, a Device Firmware Upgrade and a device debug interface (for Tiva C Series TM4C123 Launchpad). Under Windows 10 64-bit, OpenOCD will fail to connect to the debug interface.

Uninstalling the virtual com port seems to allow OpenOCD to open the ICDI interface, or using [Zadig](http://zadig.akeo.ie/) and changing the composite device host to use winusb or libusb libs is another solution. However, changing the composite host will effect the other interfaces, they will no longer show up in the device list and may be unreachable (no more VCP, no more DFU, no more JTAG) and this modification may prevent other debuggers or flash programmers from working. To revert the change the modified composite host driver needs to be removed and TI ICDI driver re-installed.

Either wipe out your VCP (no virtual com port for debug messages) or modify the composite host, these two options aren't very appealing. These work-around options didn't appeal to me, so I tried to figure out why OpenOCD was failing to open the interface.

## Testing
To test this I used python and Python-libusb1. Python-libusb1 is a python wrapper for libusb, so the functionality under windows should give the same results as how libusb is used in OpenOCD.

I put together some test cases to figure out why and where libusb or OpenOCD had an issue.

### The set up
- Install Python
- Install Python-libusb1 using pip: `pip install libusb1`
  - If pip or python isn't part of the path variables in Windows, open a command prompt and change directory to the python install directory, in Windows this might be `C:\Users\'user name'\AppData\Local\Programs\Python\Python35-32\` for python 3.5
- Get the python script: [usb_scan](https://github.com/tahull/Python-projects/tree/master/usb_scan)
- Download [libusb](http://libusb.info/) for Windows, find the `libusb.dll` for 32-bit or 64-bit Windows and copy it to the same folder with `list_dev.py`
- Run test with `py list_dev.py` (list every device libusb can see) or specify the device to list with its VID and PID `py list_dev.py 1cbe 00fd`

This test will attempt to print device information, connect to the device and print manufacture ID, Serial number, Product name, and finally claim device interfaces. This is a very simplified test similar to the listdevs.exe and xusb.exe that comes with the libusb windows package.

Using `xusb 1cbe:00fd` should print similar information about this device. However, in my case, it doesn't - it fails, which is the reason for list_dev.py.

## Test results (simplified)
Libusb has an issue talking to the first ICDI device with an error `LIBUSB_ERROR_NOT_SUPPORTED [-12]`. This was a good piece of information in seeing why OpenOCD couldn't open the ICDI interface.

```
Bus 002 Device 002: ID 1cbe:00fd
  Could not Open Device 1cbe:fd
    nb interfaces: 4
        attempting to claim interfaces on device
        could not claim:  0
        could not claim:  1
        could not claim:  2
        could not claim:  3

Bus 002 Device 002: ID 1cbe:00fd
  serial: 0E210B4D
  MF: Texas Instruments
  product: In-Circuit Debug Interface
    nb interfaces: 4    
        attempting to claim interfaces on device
        could not claim:  0
        could not claim:  1
        interface 2 claimed
        interface 3 claimed
```

## What the results mean
The ICDI appears as two devices to libusb, the first device it finds, but cannot be opened, the second device can be opened. With the first instance of the device none of the interfaces can be claimed, but on the second instance, interface 2 and 3 can be claimed. The interface we need to connect to is the debug interface which is interface 2, on the second device handle. On inspection of OpenOCD ICDI driver code, the libusb function `libusb_open_device_with_vid_pid(h->usb_ctx, param->vid, param->pid)` is used to find the ICDI and the handle usb_ctx. The device is found, but it can't be opened with the first device handle, and so it fails.

## Making it work
As is turns out there's already a better implementation in OpenOCD for finding a debug interface, its used by st-link driver stlink_usb.

ICDI driver can easily be changed to be more like the stlink_usb driver implementation.

The stlink_usb driver uses OpenOCD's libusb_common. `libusb_common.h` doesn't use libusb's function `libusb_open_device_with_vid_pid` which only searches untill the first instance. Instead a different search method is implemented in `libusb1_common.h` `(jtag_libusb_open(vids, pids, serial, &h->fd) != ERROR_OK))` which will search through all the libusb capable devices and try to to connect to the matching VID, PID or serial, if it fails on the first instance, it continues to search.

A modification is already proposed in the OpenOCD mailing [list (ID: 2527)](http://openocd.zylin.com/#/q/status:open) to change the ti_icdi_usb to use libusb_common, instead of using libusb directly, but at the time of doing my own testing, the change is not yet merged. Once merged this should be a non-issue for Windows users.

Sources of the proposed changes: [OpenOCDModFiles](https://github.com/tahull/OpenOCDModFiles)

## Testing in OpenOCD
Finally, does it work? Yes. By opening a cmd prompt with command: `openocd -f board/ek-tm4c123gxl.cfg` the debug interface can now be opened without having to un-install the VCP interface or modify the host composite device with zadig.

<img src="/images/blog/openocd/ti-icdi-success.png" class="img-fluid"/>

Notice there is an `Error: libusb_open() failed with LIBUSB_ERROR_NOT_SUPPORTED` thats ok, its just the first attempt to open the debug interface on the first device handle. Upon the second attempt, on the seconds instance of the ICDI device with the interface 2, it does connect.

## Update
Sometime ago I switched to using code composer. Initially I wanted a single Eclipse setup with support for TI launchpad tm4c, and stm32, but its just easier to use the individual pre-configured eclipse IDE's (code composer, openstm32, or atolic true studio)
