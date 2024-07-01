## Raspberry Pi

### Raspberry Pi 4

As of firmware 5.10, the HDMI interface on the Raspberry Pi 4 supports I2C.  The following steps will enable **ddcutil**. 

- Ensure you are at least on firmware 5.10, which is now mainstream.  If necessary, execute: 
<br>
`$ sudo apt update && sudo apt upgrade`
- In /boot/config/txt, look for the ***[Pi4]*** section and replace ***dtoverlay=vc4-fkms-v3d*** with ***dtoverlay=vc4-kms-v3d***
- In /etc/modules, add i2c_dev  
- Reboot  
- If you have not yet installed ddcutil: `$ sudo apt install ddcutil`
- The I2C buses for the two HDMI ports are normally /dev/i2c-11 and /dev/i2c-12. However, the bus names will be /dev/i2c-12 and /dev/i2c-13 if 
`dtparam=i2c_vc_on` is also present in config.txt.

Thank you Robert Alexa for updating the status of **ddcutil** on the Pi 4 and providing the above instructions. For a long thread on the problem of I2C support
on the Pi 4, see [Raspberry Pi issue #3152](https://github.com/raspberrypi/linux/issues/3152#)

Update 6/2023

Fresh install of Rasberry Pi OS (64 bit), build 2023-05-03 a spin of Debian 11 (bullseye) with kernel 6.1 on a Raspberry Pi Model B.   Enable I2C in raspi-config, and reboot.  apt install ddcutil, installs 0.9.9.  From OBS, installs 1.4.1.

### Raspberry Pi 3

The following instructions for installing **ddcutil** on the Raspberry Pi are a distillation of user feedback, my own experiences with a Raspberry Pi 3 Model B, 
and discussions on the Web. 


**ddcutil** is known to work on both Raspberry Pi 3 Model B and Raspberry Pi 2.  It has been tested with the following distributions:

- Raspbian (Release 2017-09-07)
- Ubuntu 16.04 
- Fedora 26
- openSUSE Tumbleweed with kernel 4.11.13 

At least on The Raspberry Pi 2 or 3, the I2C bus for the HDMI connection is /dev/i2c-2.  To enable it, the following line is required in the **config.txt** file: 
~~~
dtparam=i2c2_iknowwhatimdoing
~~~ 

Alternatively, the experimental OpenGL **vc4-kms-v3d driver** enables /dev/i2c-2 automatically.   In that case, the above ***i2c2_iknowwhatimdoing*** setting is not needed, though this may possibly change:
~~~
dtoverlay=vc4-kms-v3d
~~~


Also, ensure that i2c_dev is listed in /etc/modules, /etc/modules.conf, or a file in /etc/modules-load.d, as appropriate for your system.

Prebuilt **ddcutil** packages for the Raspberry Pi are available for many distributions.
See [Repology](https://repology.org/metapackage/ddcutil/versions) for pointers.
If there is no prebuilt version for your environment, or the prebuilt version is out of date, **ddcutil** can be built from source.



### Background

Enabling /dev/i2c-2 requires the peculiar dtpararm statement described above, instead of the more expected: 
~~~
# Won't work!
dtparam=i2c2
~~~

The parameter name was chosen as a caution.  From the [commit](https://github.com/raspberrypi/linux/commit/c6f0998f61b279d47e210b90d645643a6fe7932b) that added the option: 

> The third I2C bus (I2C2) is normally reserved for HDMI use. Careless use of this bus can break an attached display - use with caution.

> It is recommended to disable accesses by VideoCore by setting hdmi_ignore_edid=1 or hdmi_edid_file=1 in config.txt.

> The interface is disabled by default - enable using the i2c2_iknowwhatimdoing DT parameter.

The issue appears to be that application access to /dev/i2c-2 from the CPU is independent of, and so can conflict with, video driver access via the GPU (at least with the default driver).  From an [online comment](https://github.com/raspberrypi/linux/issues/1028): 

> Yes, but the GPU considers itself to be the sole user of the BSC2 peripheral. Poking the i2c from the ARM CPU (via linux driver) will likely work, but beware if the GPU tries to access the same peripheral then bad things will happen.
. As long as you stay away from hotplugging HDMI, or tvservice commands (or anything likely to use tvservice) then the GPU is unlikely cause any traffic on BSC2.

As a practical matter, this does not appear to be an issue.

Concerns have also been expressed about 3.3v vs 5v voltage incompatibility on the I2C bus, since the HDMI spec is 5v.  However, as I read the 
[I2C Specification](https://www.nxp.com/docs/en/user-guide/UM10204.pdf) any recent I2C implementation should be able to adapt to voltages up
to 5.5 volt. Again, I have not experienced a problem in this regard. 

