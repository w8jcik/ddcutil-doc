

## **ddcutil** Configuration

Several aspects of the communication environment must be configured.  


### Ensure the monitor is enabled for DDC communication

Nearly all monitors manufactured since the mid 2000's support DDC/CI, as do many made earlier.  For most monitors that
support DDC/CI, whether DDC/CI comminication is actually enabled is controlled by a setting in the on screen display.
The item is typically found in a submenu named something like "Other Settings"

### Video Driver

The video driver must support I2C bus communication.   All major open source drivers (nouveau, radeon, admdgpu, i915) support 
I2C bus communication, as does the Nvidia proprietary driver.  

If using Nvidia's proprietary video driver, special settings may be necessary.  See [Special Nvidia Driver Settings](nvidia.md)

For the Rapberry Pi, see the section [Raspberry Pi](raspberry.md).

### Ensure that the /dev/i2c-N devices exist

Typically, the I2C buses are exposed as devices named /dev/i2c-N. 
Kernel module i2c-dev must be loaded to create the /dev/i2c-N devices.  See [Kernel Module Configuration](kernel_module.md)

### Grant read/write permission for the /dev/i2c-N devices representing monitors

**ddcutil** users require read/write permission to /dev/i2c-N devices.  With varying complexity, this can be effected in many ways.

- Run **ddcutil** as root.  

- Grant everyone RW access to the /dev/i2c devices for the current boot.

~~~
$ sudo chmod a+rw /dev/i2c-*
~~~

- Assign the /dev/i2c devices to a group (normally i2c) and assign users who will run **ddcutil** to that group.
  For details, see [I2C Device Permissions](i2c_permissions.md).

<!--
Add a file with the following line to /etc/udev/rules.d:

~~~
SUBSYSTEM=="i2c-dev",KERNEL=="i2c-[0-9]*", MODE="0666"
~~~

- If /dev/i2c-N devices are already assigned to group i2c, add users to that group. 

This is the case, for example, if package i2c-tools has been installed on a Debian derived distribution (e.g. Ubuntu).  In that case,
file /lib/udev/rules/60-i2c-tools.rules is installed.

Add the users who will run **ddcutil** to that group. 

~~~
usermod -G i2c -a <yourname>
~~~

- Create group i2c to manage access.

The most general case entails creating group i2c, creating a UDEV rule assigning /dev/i2c-N devices to group i2c, and adding users to group i2c. 

-->

- USB connected monitors

For details about USB device permissions for those monitors using USB instead of I2C to communicate the Monitor Control Command Set, 
see [Device Permissions](i2c_permissions.md).


<a name="envcmd"></a>
## Installation Diagnostics

If ddcutil installs successfully but execution fails, command `ddcutil environment`
can be used to probe the I2C environment and may provide clues as to the problem.
For USB connected monitors, use command `ddcutil usbenvironment`.

Linux command `i2cdetect` (typically found in package **i2ctools**) provides an
independent check of whether the DDC slave address (X37) is active on an I2C bus.
