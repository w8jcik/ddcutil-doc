### Command **setvcp**

Syntax: **setvcp** &lt;display-selection-options> (feature-code) [+|-] new-value) *

In full generality, the **new-value** argument to **setvcp** is a 2 byte number. A handful of monitors have continuous features
with values greater than 255.  Also, the settable VCP feature x73 (Gamma) has a complex 2 byte encoding.  Other than these exceptional cases,
all settable values are integer values in the range 0..255, i.e. a single byte.

Values can be specified either in decimal form, or as 1 or 2 byte hex numbers, with a leading "x" or "0x". Examples:

~~~
$ ddcutil setvcp 10 50
$ ddcutil setvcp 10 0x32
$ ddcutil setvcp 73 x5304
~~~

Multiple ***feature***/***new-value*** pairs can be given on a single **setvcp** command.  For example: 
~~~
$ ddcutil setvcp 10 50 12 75
~~~

It is possible to specify a relative instead of absolute value for a continuous feature by putting " + " or " - " between the feature id and value.

- **The plus or minus signs must surrounded by spaces to indicate a relative value operation.**  
- The new value is adjusted so that it is never less than 0 or greater than the maximum value of the feature.
- In this situation **setvcp** first reads the current value before writing the adjusted value.

For example, to adjust the brightness value up or down by 5:
~~~
$ ddcutil setvcp 10 + 5
$ ddcutil setvcp 10 - 5
~~~

#### Option: ***--permit-unknown-feature***<a name="option_permit_unknown_feature"></a>

Normally, **setvcp** does not allow setting the value of an unrecognized feature.  Use this option to allow **setvcp** to set the value of any feature.

#### Option: ***--noverify***<a name="option_noverify"></a>

When **ddcutil** sends a **Set VCP Feature** packet to a display, it receives an acknowlegement at the I2C level that the request packet was received.
There is no acknowledgement that the display has successfully executed the command.
There have been occasional reports that the **setvcp** command appeared to succeed, but the new value was not set.

Therefore **setvcp** (and **loadvcp**) by default perform a **getvcp** operation
to verify that the new value has actually been set.
(Obviously, this verification does not occur for write-only features.  It also does not occur for features such as x60 (Input Source) for which
verification can be expected to fail.) 

For most monitors, this verification is unnecessary.  To improve performance it is possible to skip value verification using the ***--noverify*** option. 


### Option: ***--verbose***

Display informational messages regarding verification.

### Option ***--mccs &lt;vcp version>***<a name="option_setvcp_mccs"></a>

Parse the new value based on the the specified MCCS (aka VCP) version, instead of the version obtained from feature xDF (VCP Version).