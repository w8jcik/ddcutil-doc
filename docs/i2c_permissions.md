## I2C Device Permissions<a name="i2c_permissions"></a>

**ddcutil** requires read/write access to /dev/i2c devices, or more precisely all /dev/i2c devices associated with video cards. It can, of course, be run as root.

(For USB devices, see [USB Device Permissions](usb_permissions.md)).

Starting with release 1.4.0, **ddcutil** installed udev rule 60-ddcutil.rules that grants read/write access to the logged on user.

As of release **ddcutil** 2.0, this has changed to files 60-ddcutil-i2c.rules and  60-ddcutil-usb.rules. 

### Granting I2C Device Permissions on Versions Prior to 1.4.0

On Debian, Ubuntu, and likely other Debian derived distributions, most of the work has already been done.
Package ddcutil requires package i2c-tools. Installation of package i2c-tools creates group ***i2c*** (if it does not alreay exist), and assigns that group to the
/dev/i2c devices using a udev rule. 

All that is necessary is to add users who will use **ddcutil** to group ***i2c***: 
~~~
$ sudo usermod <user-name> -aG i2c
~~~

On other than Debian derived distributions, additional steps may be necessary. 

If group ***i2c*** does not already exist, the following command will create it: 
~~~
$ sudo groupadd --system i2c
~~~

Then add your userid to group ***i2c*** as above.

A sample udev rule for giving group ***i2c*** RW permission on the /dev/i2c devices 
can be found in distributed file **45-ddcutil-i2c.rules**.  Its exact location varies by distribution, but 
commonly the file is found in directory /usr/share/ddcutil/data.  The file can be copied to 
/etc/udev/rules.d, but do check that this rule does not conflict with others in that directory.
~~~
$ sudo cp /usr/share/ddcutil/data/45-ddcutil-i2c.rules /etc/udev/rules.d
~~~

For testing, it may be simpler to give everyone permission to write to 
/dev/i2c-* for the current boot:
~~~
$ sudo chmod a+rw /dev/i2c-*
~~~

