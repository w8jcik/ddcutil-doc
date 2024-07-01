## Special Nvidia Driver Settings

When using Nvidia's proprietary driver, I2C communication fails on some cards.
This has been the topic of some discussion on Nvidia's web site.  See, 
[for example](https://devtalk.nvidia.com/default/topic/572292/linux/-solved-does-gddccontrol-work-for-anyone-here-nvidia-i2c-monitor-display-ddc/3).

While not a universal solution, setting undocumented Nvidia options sometimes enables I2C communication to work.  There are several ways 
to set these options.  The simplest, outlined in a [post by user arcnmx](https://devtalk.nvidia.com/default/topic/572292/linux/gddccontrol-issues-with-nvidia-drivers-i2c-monitor-display-ddc-dp-hdmi-failing/post/5239597/#5239597), puts the settings in /etc/modprobe.d. Add a file whose name ends in ".conf" to that directory, 
e.g. /etc/modprobe.d/nvidia_i2c.conf. The file should contain the following:
~~~
options nvidia NVreg_RegistryDwords=RMUseSwI2c=0x01;RMI2cSpeed=100
~~~

After rebooting, confirm that the settings have been applied:
~~~
    $ grep RegistryDwords /proc/driver/nvidia/params
    RegistryDwords: "RMUseSwI2c=0x01;RMI2cSpeed=100"
~~~

An [earlier version](https://www.ddcutil.com/nvidia_old) of this page discussed how to set the options using a file in /etc/X11/xorg.conf.d.  It has been retained for reference as it may be useful if
the /etc/modprobe.d method is not suitable for your setup.
