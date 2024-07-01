## Display Selection

With the exception of **detect**, all primary **ddcutil** commands operate on a single monitor.
If there are multiple DDC capable monitors, one must be selected. 

Display detection, which is explicitly reported by [**ddcutil detect**](command_detect.md), assigns each MCCS capable monitor a display number, 
starting from 1.  First are the I2C connected displays in I2C bus order, followed by any displays using
USB for communication.  If no display is specified on the command line, the first display is chosen. 

If more than one monitor is attached, the desired monitor can be specified 
using the following options:

| Option                                  |  Note                                |
|-----------------|----------------------------------|
| ***--display*** &lt;display number>           |
| ***--bus*** &lt;i2c bus number>               | alt: ***-b*** &lt;i2c bus number>
| ***--usb*** &lt;usb bus number>.&lt;usb device number>
| ***--hiddev*** &lt;usb hiddev device number>  |
| ***--edid*** &lt;256 character hex string>    | 
| ***--mfg*** &lt;mfg code>                     |  3 character mfg id from EDID |
| ***--model*** &lt;model name>                 |  model name from EDID
| ***--sn*** &lt;serial number>                 |  ASCII serial number from EDID

<br>
Any combination of manufacturer code, model name and serial number can be used together to identify a monitor.
The first monitor to satisfy all the specified criteria is selected.

To see a list of all attached monitors and their associated identifiers:
~~~
$ ddcutil detect
~~~

See also: [ddcutil detect example](detect_verbose_output.md)
