## Post-Installation Checklist 

This page outlines steps that may be required to configure **ddcutil** after it has been installed.

### Kernel Module i2c-dev 

**ddcutil** requires kernel module i2c-dev.  If it is not built into your kernel, it must be loaded explicly.
To do so, add a file into directory /etc/modules-load.d with the single line: 

~~~
i2c-dev
~~~

As of release 1.4, **ddcutil** installation should automatically install this file, making manual configuration unnecessary.

For details, see [Kernel Module Configuration](kernel_module.md). 

### I2C Device Permissions

Permissions must be granted in order for users other than root to run **ddcutil**.

As of release 2.0, **ddcutil** installation automatically installs files /usr/lib/udev/rules.d/60-ddcutil-i2c.rules and /usr/lib/udev/rules.d/60-usb.rules.  
Using tag **uaccess**, these rules grant the logged on user read/write access to /dev/i2c devices associated with monitors.

For releases prior to 2.0, or if installing on a system that does not support udev (i.e. does not run systemd), manual configuration may be required.
For details, see [I2C Device Permissions](i2c_permissions.md).

<!--
On some distributions, including Debian and Ubuntu, all /dev/i2c devices are assigned to group **i2c** by installation of required package **i2c-tools**.  All that is necessary is to add **ddcutil** users to group **i2c**: 

~~~
$ sudo usermod <user-name> -aG i2c 
~~~

If installing from some other distribution or from source, additional configuration may be required.  See [I2C Device Permissions](i2c_permissions.md).
-->

### Proprietary Nvidia Driver

The proprietary Nvidia video driver sometimes requires special settings for I2C communication.  See [Special Nvidia Driver Settings](nvidia.md).

## Configuration Diagnostics

If ddcutil installs successfully but execution fails, command `ddcutil environment`
can be used to probe the I2C environment and may provide clues as to the problem.
For USB connected monitors, use command `ddcutil usbenvironment`. See [Secondary Commands](secondary_commands.md).

Linux command `i2cdetect` (found in required package **i2ctools**) provides an
independent check of whether the DDC slave address (X37) is active on an I2C bus.


### Shared Library Configuration

When installing from source, additional configuration may be required to use shared library **libddcutil** if it is installed under /usr/local.  See [Shared Library Configuration](shared_lib_config.md).  Note that **libddcutil** is NOT required to run **ddcutil**. 

### Additional Information

See also [**ddcutil** Configuration](config.md).
