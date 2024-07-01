## User Defined Features

***User Defined Features*** make it possible to modify the feature definition obtained from the ***Monitor Control Command Set*** specification, 
or to create a new one entirely.
A common use of this facility is to replace the list of valid values of a simple non-continuous feature.
***User Defined Features*** is enabled by command line option ***--enable-udf*** (alt.***--udf***). 
It is desiabled by option ***--disable-udf*** (alt. ***--noudef***).
The default is ***--enable-udf***. 

The user defined features for a monitor are specified in a feature definition file. The format of this file is subject to change.

### User Defined Feature File

Here's an example of a file that specifies user defined features. 
~~~
$ cat ~/.local/share/ddcutil/DEL-DELL_U2222-16485.mccs
MFG_ID       DEL
MODEL        DELL U2222
PRODUCT_CODE 16485
MCCS_VERSION 2.2
FEATURE_CODE FD  Feature Name
    ATTRS NC RW
    VALUE 0 First Val
    VALUE 0X01 Second Val 0x
    VALUE X02  Third value X
    VALUE 03H  H at end
FEATURE_CODE xFB Another Feature
    ATTRS C RO
FEATURE_CODE 0xfa a feature with code staring with 0x
    ATTRS C RO 
~~~
The first 3 lines identify the monitor model, using a 3 character manufacturer id, the model name, 
and the integer product number, as given in the EDID, and reported by the **detect** command.
The feature definition applies to any monitor that matches those identifiers. 
Model name and product number are both required because some manufacturers use generic model names, 
and many set the product number to 0. 
For example, Samsung commonly uses the name "SyncMaster", and LG commonly uses the name "LG ULTRAWIDE".



The optional ***MCCS_VERSION*** line specifies the version of the MCCS Specification used to describe the monitor.
If specified, it is used instead of the value returned by querying VCP feature xDF (VCP Version). 
If the MCCS version is also specified using the ***--mccs*** command  line option, the command line option takes precedence.

In addition to the monitor identification lines and optional MCCS version line, there are one or more feature definitions.  

Each defnition starts with a line of the form ***FEATURE_CODE &lt;feature-id> &lt;feature name>***.
Feature codes are implicitly hex numbers, and can be written with or without a leading "x". 

The ***ATTRS*** line contains feature attributes: "C" or "NC" indicating whether 
the feature is continuous or non-continuous, and "RW", "RO", or "WO" indicating whether 
the feature is read-write, read-only, or write-only. 

For non-continuous features, there are one or more ***VALUE*** lines describing the possible values. 
Each line has the form: ***VALUE &lt;value> &lt;description>***.  Feature values can be written either 
as decimal or hexadecimal. 

The MCCS specification has evolved to include features that don't neatly conform the original Continuous/Non-Continuous classification.  For example, continous features can have special reserved values, and non-continuous features exist that do not have simple tables of feature values, but instead have unique interpretations.  The ***User Defined Feature*** facility
cannont define such features.

Blank lines and lines beginning with "\*" or "#" are ignored.

### Examples

An actual feature definition file for a monitor whose values for feature x60 (Input Source) do not conform to the MCCS specification. 

~~~
$ cat ~/.local/share/ddcutil/SAM-U32H75x-3586.mccs
MFG_ID       SAM
MODEL        U32H75x
PRODUCT_CODE 3586
MCCS_VERSION 2.1
FEATURE_CODE 60 Input Source
  ATTRS NC RW
  VALUE x05 HDMI-1
  VALUE x06 HDMI-2
  VALUE x0f DisplayPort
~~~

### Feature Definition File Name and Search Path

The manufacturer code, model name, and product code are used to create the file name of the feature definition file, 
which has the form  ***&lt;mfg_id>-&lt;model name>-&lt;product code>.mccs***.
For manufacturer id and product code, the values are exactly as obtained from the EDID and given on the MFG_ID and PRODUCT_CODE lines 
of the feature definition file.
For the model name (as given on the MODEL line, any non-alpanumeric character in the name, such as a blank, is replaced by "_" (an underscore) in the file name.


Feature definition files are saved in the user's ***ddcutil*** data directory, $HOME/.local/share/ddcutil. 
More generally, ***ddcutil*** searches for feature definition files using environment variables ***$XDG_DATA_HOME*** and ***$XDG_DATA_DIRS***, per the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html).

If dynamic features are enabled and option ***--verbose*** is specified, commands **capabilities**, **getvcp**, **setvcp**, **dumpvcp**, and **probe** report the fully qualified name
of the feature definition file (if it was found), or the simple file name (if it was not found).  As of release 2.1.5, command **detect** also reports the name.
