## USB Device Permissions<a name="usb_permissions"></a>

If using a monitor that transmits MCCS over USB, i.e. a monitor that provides access to its Virtual Control Panel over USB, **ddcutil** requires read/write access to device
/dev/usb/hiddev**N**, where **N** is the number for the device representing the monitor's USB connection.

**ddcutil** has to be careful when accessing the /dev/usb/hiddev* devices, as some of these 
can represent USB attached input devices such as keyboards or mice. 

Distributed file **45-ddcutil-usb.rules** shows a couple ways to give **ddcutil** the required 
access.  All but one of the command lines in that file should commented out.
This file can be edited and copied to 
/etc/udev/rules.d, but do check that the rules in this file do not conflict with others in that directory.
~~~
$ sudo cp /usr/share/ddcutil/data/etc/udev/rules.d/45-ddcutils-usb.rules /etc/udev/rules.d
~~~

The following rule gives all users access to the Virtual Control Panel of an Apple Cinema Display by specifying its 
USB device user id (aka uid) and product id (aka pid):
~~~
SUBSYSTEM=="usbmisc", ATTRS{idVendor}=="05ac", ATTRS{idProduct}=="9223",  MODE="0666" 
~~~

This line would of course have to edited for your monitor.  One way to find its uid/pid is the 
**lsusb** command. 

A more robust way to set proper device permissions is to call **ddcutil** from a udev rule 
to test whether a device is a HID compliant monitor: 
~~~
SUBSYSTEM=="usbmisc",  KERNEL=="hiddev*", PROGRAM="/usr/bin/ddcutil chkusbmon $env{DEVNAME} -v", MODE="0660", GROUP="video"
~~~
Note that the path to the ddcutil executable will have be edited to the location where ddcutil is installed on your system. 
(For details on command **ddcutil chkusbmon**, see [here](usb.md#command_chkusbmon).

The -v option produces informational messages.  These are lost when the rule is normally executed by
udev, but can be helpful when rules are tested using the **udevadm test** command. 


Bear in mind when looking at alternative udev rules is that a device can be be identified in several different ways, 
two of which are shown above.  Pick one that best works for you.

The following section from the udev documentation 
(<https://www.kernel.org/pub/linux/utils/kernel/hotplug/udev/udev.html>) 
may be helpful:

> The udev rules are read from the files located in the system rules directory 
> /usr/lib/udev/rules.d, the volatile runtime directory /run/udev/rules.d and 
> the local administration directory /etc/udev/rules.d. All rules files are 
> collectively sorted and processed in lexical order, regardless of the directories 
> in which they live. However, files with identical file names replace each other. 
> Files in /etc have the highest priority, files in /run take precedence over files 
> with the same name in /lib. This can be used to override a system-supplied rules 
> file with a local file if needed; a symlink in /etc with the same name as a rules 
> file in /lib, pointing to /dev/null, disables the rules file entirely.
