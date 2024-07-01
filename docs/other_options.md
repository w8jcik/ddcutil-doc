### Miscellaneous Options

#### Option: ***--help***

Displays command help, then terminates execution.

#### Option ***-hh***<a name="option_hh"></a>

The number of command options have become voluminous.  Option ***--help*** reports only those that are likely to be of interest to most users.
If option ***--hh*** is specified, then all options recognized by the parser, including ones used for development and debugging, deprecated options
that currently have no effect but which are recognized so as not to break existing scripts, and alternative option names.


<!--
#### Option: ***--timeout-i2c-io***

Causes (most) I2C read calls to eventually time out if they do not return.
This has been implemented as a POSSIBLE way to address occasional reports of **ddcutil** locking up, with subsequent
 **ddcutiil** calls blocking behind it.   it may become the default in future releases.
-->

#### Options: ***--enable-udf***, ***--disable-udf***<a name="option_enable_udf"></a>

Enable or disable the user defined feature facility.  The default is ***--enable-udf***. 


#### Option ***--mccs &lt;vcp version>***

Option ***--mccs*** forces the MCCS version number.  It applies to commands **vcpinfo**, **getvcp**, **setvcp**, and **dumpvcp**.
See documentation of the individual commands for details.

#### Option: ***--version***

Displays **ddcutil** version information, then terminates execution.

#### Option: ***--settings***

Report current option settings at the start of program execution.



### Option: ***--noconfig***

Do not process the configuration file.
