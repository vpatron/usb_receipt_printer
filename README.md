# Printing directly to a POS58 USB Thermal Receipt Printer using Linux/Raspberry Pi

This is a demo program to print to the POS58 USB thermal receipt printer using Python under Linux
(tested on a Raspberry Pi). This is printer labeled under different companies, but is made by 
Zijiang. See http:zijiang.com.

This has been tested on LinuxMint 18, a Raspberry Pi 3 B+, and a Raspberry Pi Zero W (pictured below).

![USB Thermal Receipt Printer with Raspberry Pi](POS58-receipt-printer-raspberry-pi.jpg "USB Thermal Receipt Printer with Raspberry Pi")

# Don't use Zijian Linux Driver

Their Linux driver has problems:

1. Their installer connects the printer to the wrong port so it never prints. (You need to go to CUPS manager and
fix the port path.)

2. Their driver uses the printer in graphics mode and their font rendering looks really terrible for small text.

3. Using CUPS adds unecessary complication, like adding default margins

# Direct printing

This demo program opens the USB port directly and writes directly to the USB endpoint of the printer device.
This turns it into a very simple text printer. You can include Epson ESC/POS sequences yourself to get fancy.

# Linux Configuration

Special configuration is usually handled by an installation script. I offer no install script, but this is
easy to do manually:

1. Add your user to the Linux group "lp" (line printer), otherwise you will get a user 
permission error when trying to print.

```
    sudo addgroup <myusername> lp
```

2. Add a udev rule to allow all users to use a USB device that matches this vendor ID and product ID.
Without this rul, you will get a permissions error.

Example: in `/etc/udev/rules.d` create a file ending in .rules, such as `33-receipt-printer.rules` with the contents:

```
    # Set permissions to let anyone use the thermal receipt printer
    SUBSYSTEM=="usb", ATTR{idVendor}=="0416", ATTR{idProduct}=="5011", MODE="666"
```

# Python setup

This code is mainly from the example code at [https://github.com/pyusb/pyusb](https://github.com/pyusb/pyusb)

See that site to install pyusb and a USB backend. This was tested using usblib 1.0. Quick reference:

Install pyusb to your Python (suggest using virtualenv):

```
pip install pyusb
```

Install libusb 1.0:

```
sudo apt install libusb-1.0-0-dev
```
And that's it. Happy printing!

# Other projects

See my blog for other interesting projects at [https://vince.patronweb.com](https://vince.patronweb.com)
