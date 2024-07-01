## Configuration File<a name="permissions_file"></a>

Most **ddcutil**, **libddcutil**, and **ddcui** options, other than those for display or feature selection,
can be specified in a configuration file.
Originally, command line options for **ddcutil**, **ddcui**, and library **libddcutil**
were specified in a single configuration file, **ddcutilrc**.  As of release 2.0, 
**ddcui** command line options are specified in a separate configuration file, **ddcuirc**. 

**ddcutil** first looks for file **ddcutil/ddcutilrc** (or **ddcutil/ddcuirc**) in the XDG_CONFIG_HOME directory, 
and then on the XDG_CONFIG_DIRS search path.  Normally this resolves to 
$HOME/.config/ddcutil/ddcutilrc
   
***ddcutilrc*** is a standard style INI file,
It has the following sections: 

| Section name | Purpose
|--------------|---------
| [global]     | &nbsp; Always apply
| [ddcutil]    | &nbsp; Applies to command **ddcutil**
| [ddcui]      | &nbsp; Applies to command **ddcui**
| [libddcutil] | &nbsp; Used to configure shared library **libddcutil**

<br>
Within each section, key ***options*** specifies an option string. 

Command options for monitor or feature selection cannot be specified.

For **ddcutil** and **ddcui**, the options string obtained from the 
configuration file is used to prefix the command line options. 

For **libddcutil**, the option string is read directly when the shared 
library is initialized. 

For example: 

~~~
# This is a comment line

[global]
; Combined with all other options strings
options : --edid-read-size 256

[ddcutil]
   * This is a comment too
   options : --sleep-multiplier .5 --maxtries "3,5,3"

[libddcutil]
   ; And this is a comment.  Leading spaces in a line are ignored
   options: --noverify
~~~

Section names and keys are case insensitive
