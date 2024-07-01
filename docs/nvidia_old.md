### Special Nvidia Driver Options in X11

This is a slightly edited version of an older version of page [Special Nvidia Driver Settings](nvidia.md) describing how to apply the special Nvidia driver options within X11.
It may be useful to those for whom adding a file to modprobe.d does not work.

When using Nvidia's proprietary driver, I2C communication fails on some cards. 

It works on several older Nvidia cards I have, but failed with my more recent 
GTX660Ti. (Specfically, I2C reads of 1 or 2 bytes succeeded, but reads of 3 
or more bytes failed.)  Others have reported similar problems. 

Per [this discussion](https://devtalk.nvidia.com/default/topic/572292/linux/-solved-does-gddccontrol-work-for-anyone-here-nvidia-i2c-monitor-display-ddc/3), adding the 
following to the "Device" section for the Nvidia driver resolves the problem for some cards:
~~~
 Option     "RegistryDwords"  "RMUseSwI2c=0x01; RMI2cSpeed=100"
~~~
 A file for making this change is 90-nvidia_i2c.conf.  Its exact location varies by where a distribution installs an application's data files. Commonly it can be found in directory /usr/share/ddcutil/data.  
~~~
Section "Device"
   Driver "nvidia"
   Identifier "Dev0"
   Option     "RegistryDwords"  "RMUseSwI2c=0x01; RMI2cSpeed=100"
   # solves problem of i2c errors with nvidia driver
   # per https://devtalk.nvidia.com/default/topic/572292/-solved-does-gddccontrol-work-for-anyone-here-nvidia-i2c-monitor-display-ddc/#4309293
EndSection
~~~
Copy this file to /etc/X11/xorg.conf.d.  You will have to restart X11 for this change to take effect.  It's probably simplest to reboot.

Note: This file works if there is no /etc/X11/xorg.conf file.  If you do have an 
xorg.conf file the Identifier field will likely require modification.


This does not, however, appear to be a universal solution.  It does not work with some cards. 

In a [related post on Nvidia's site](https://devtalk.nvidia.com/default/topic/572292/linux/gddccontrol-issues-with-nvidia-drivers-i2c-monitor-display-ddc-dp-hdmi-failing/post/5239597/#5239597), 
user arcnmx reported:

> I hadn't seen it suggested yet, so for anyone using KMS/nvidia-drm or otherwise loading the nvidia kernel module early on who can't get the xorg.conf setting to fix DDC on newer cards, I've had to add the following to modprobe.conf or modprobe.d/whatever.conf instead:

~~~
options nvidia NVreg_RegistryDwords=RMUseSwI2c=0x01;RMI2cSpeed=100
~~~

> To confirm that the settings are working: 
~~~
    $ grep RegistryDwords /proc/driver/nvidia/params
    RegistryDwords: "RMUseSwI2c=0x01;RMI2cSpeed=100"
~~~

> This makes DDC over HDMI work with my Pascal card and fixes the "invalid EDID" errors at least, and also doesn't require starting X to use it, should help with wayland, etc. 

