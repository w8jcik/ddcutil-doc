## Command capabilities

Syntax: **capabilities** &lt;display-selection-options> &lt;general options>


Reports the monitor's capabilities string.  This string describes, among other things, the VCP Features that the
monitor supports and, for non-continuous features where appropriate, the allowed values.

Unfortunately, the capabilities string is unreliable. Therefore **ddcutil** simply presents the string.
It does not use the capabilities string when formulating commands and 
interpeting responses.  

The capabilities string is unreliable for several reasons: 

- The response to the DDC capabilities request may simply be incorrect.   For example, the capabilities response from the HP LP2480zx
monitor does not list VCP feature code x10 (brightness) as supported.  However, feature code x10 is in fact supported.  The only 
way to know for sure if a monitor implememts a VCP feature code is to try it.

- A DDC capabilities request requires multiple I2C exchanges, making it both slow and unreliable.  Because of its complexity, it is particularly vulnerable to
I2C errors.  On monitors with paticularly poor I2C implementations it sometimes fails because the maximum number of 
I2C retries is exceeded.

- There is no capabilities request defined in the programming interface for USB connected monitors. 
ddcutil simulates a capabilities response by looking at what USB HID reports exist for feature codes. 

Note however, that **ddcui** does make use of the the capabilities string.

By default the **capabilities** command displays the capabilities string in parsed form. 

#### Option: ***--terse***

Display the unparsed capabilities string. 

#### Option: ***--verbose***

Display both the unparsed string and its parsed form.