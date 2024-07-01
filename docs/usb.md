## USB Connected Monitors

**ddcutil** supports monitors that implement the Monitor Control Command Set (MCCS) over USB, 
following the [USB Device Class Definition for Human Interface Devices (HID)](https://usb.org/document-library/device-class-definition-hid-111)
) and the [USB Monitor Control Class Specification](https://usb.org/sites/default/files/usbmon10.pdf).

Note that some monitors use USB to communicate, but do not follow the specification for MCCS over USB. 
For example, the HP Dreamcolor 2480zx uses a proprietary, undocumented protocol. 

Also, bears emphasizing that when **ddcutil** refers to USB connected monitors, it means monitors that communicate
MCCS over USB, **NOT** to monitors that communicate the video signal over USB.

Limitations:   
- The MCCS over USB specification has no mechanism for querying a capabilities string.  The **ddcutil capabilities** 
command synthesizes a response based on the feature codes it detects.   
- Table type features are completely unsupported.  
- Apple Thunderbolt displays might work, but are not supported.  Thunderbolt integrates video, USB, and ethernet into a single cable, and 
appears to require a Thunderbolt capable computer, as I can find no adapters that split out all the signals.
Other than actual Macs, these are uncommon.

On the other hand, MCCS over USB has advantages:  
- Unlike I<sup>2</sup>C, the protocol is inherently reliable.  No retry logic is needed.  
- Unlike I<sup>2</sup>C, USB communication does not require waits between system calls.  It is therefore faster. 

Thanks to Ojdrej Zary, whose [usbmonctl](https://github.com/ondrej-zary/usbmonctl) was used as the basis for USB support in **ddcutil**.

If there are multiple monitors, a USB connected monitor can be selected in the usual manner using its display number (option ***--display***), or by
its bus number/device number pair (option ***--usb***) or hiddev device number (option ***--hiddev***).  

There are also 2 **ddcutil** commands specific to USB connected monitors, **chkusbmon**, and **usbenvironment**c

For details on USB device permissions, see [USB Device Permissions](usb_permissions.md).

### Options for Display Selection

#### Option: --usb<a name="option_usb"></a>


USB connected monitors can be specified by their USB bus number and device number.  The numbers are separated by either a period or colon.

For example: 

~~~ 
ddcutil --usb 3.5 ...
~~~

selects the monitor at USB bus number 3, device number 5.  


#### Option --hiddev<a name="option_hiddev"></a>

USB connected monitors can also be specfied by their hiddev device number, which specifies the /dev file 
by which they are accessed. That is, **--hiddev 2** refers to /dev/usb/hiddev2. 


### Command **ddcutil chkusbmon**<a name="command_chkusbmon"></a>

Command **ddcutil chkusbmon** helps to detect USB HID compliant monitors.  It is intended for use in udev rules.

It uses Read-Only access to check that a USB device is of class User Interface Device, but is not a keyboard or mouse.   
~~~
ddcutil chkusbmon ***hiddev device name***
~~~

e.g. 
~~~
ddcutil chkusbmon /dev/usb/hiddev3 
~~~

Returns 0 if a device represents a USB attached monitor, non-zero if not.  See [Device Permissions](i2c_permissions.md)

### Command **ddcutil usbenvironment**<a name="command_usbenv"></a>

Explores the system's USB environment as an aid to diagnosing USB device detection and communication issues.

Option ***--verbose*** increases the amount of information. 

The word "usbenvironment" can be abbreviated down to its first 6 letters, e.g. 
~~~
$ ddcutil usbenv
~~~
