### Options For Driver i2c-dev

#### Option: ***--use-file-io***<a name="option_use_file_io"></a>

Driver **i2c-dev** provides two interfaces for applications.  The first uses ioctl().  The second, simpler, interface hides the ioctl() calls behind a traditional file io based interface, using functions write() and read().

Normally, **ddcutil** uses the ioctl() interface. Unfortunately, when the ioctl() interface is used with older versions of the Nvidia proprietary video driver, the Nvidia driver rejects calls from i2c-dev. When this error is detected, **ddcutil** switches to using the file io interface.

This option forces use of the file io interface, avoiding the need for recovery.

For details about how this problem was diagnosed, see [ddcutil issue #341](https://github.com/rockowitz/ddcutil/issues/341).

<!--
#### Option: --use-ioctl-io
-->

#### Option: ***--force-slave-address***<a name="force_slave"></a>

<!--
Per the DDC specification, ***ddcutil*** reads the EDID over an I<sup>2</sup>C bus using slave address x50, and performs DDC communication
using slave address x37.  
-->

When the i2c-dev file io interface is used, occasionally communication cannot be established because driver i2c-dev has marked the I<sup>2</sup>C bus   as being in use by another program (error **EBUSY**).
This option allows **ddcutil** to take control of the bus, potentially impacting the other program.
(Internally, **ddcutil** uses ioctl(I2C_SLAVE_FORCE) instead of ioctl(I2C_SLAVE).)

This option has no effect in the normal case where the i2c-dev ioctl() interface is used.

#### Option: ***--edid-read-size 128|256***<a name="option_edid_read_size"></a>

Sometimes a display's EDID cannot be read on I<sup>2</sup>C slave address x50.  As a result, 
ddcutil does not report the existence of a display connected to the /dev/i2c device.
***ddcutil*** attempts to work around this by trying to read the EDID multiple times with both 128 and 256 block
sizes. Depending on the monitor, usually one or the other will work, but sometimes neither do.
Option ***--edid-read-size***, introduced in release 1.0.1, can force the block size
to 128 or 256.

#### Option: ***--i2c-source-address &lt;hex address>***<a name="option_source_address"></a>

Recent LG monitors allow for splitting the display screen between multiple input sources.
Instead of the (relatively) simple VCP feature x60 (Input Source),
they use an undocumented protocol,
to control how the screen is split and which input source is shown where.  Thanks to a community 
effort, particularly by user Max Thomas (Github handle [shinyquagmire23](https://github.com/shinyquagsire23)),
some of this protocol has been reverse engineered. 
It entails using an alternative I<sup>2</sup>C source address, x50 instead of x51, in the I<sup>2</sup>C packet. 
Option ***--i2c-source-addr*** provides a way to use this alternative address in DDC communication.
For further details, see this [wiki page](https://github.com/rockowitz/ddcutil/wiki/Switching-input-source-on-LG-monitors).
The extended discussion summarized on the wiki page occurred on
[ddcutil issue 100: LG 29UM69G fails switching input](https://github.com/rockowitz/ddcutil/issues/100).
