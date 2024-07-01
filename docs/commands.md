##Command Overview

**ddcutil** has the following commands:

| Primary Command              | Function 
|---------------------------------|--------------------------------------------------------------|
| detect                          | report monitors detected                                     |
| capabilities                    | report a monitor's capabilities string                       |
| getvcp feature-code-or-group    | report a single VCP feature value, or a group of values      |
| setvcp feature-code new-value   | set a single VCP feature value                               |
| vcpinfo (feature-code-or-group)&nbsp; | list VCP features codes that ddcutil knows how to interpret  | 
| dumpvcp filename                | save color related VCP feature values to a file              |
| loadvcp filename                | restore color related VCP feature values from a file         |
| scs                             | execute DDC/CI Save Current Settings operation                |


<br>

| Secondary Command  | Function |
|---------------------------------|--------------------------------------------------------------|
| environment                     | explore the ddcutil installation environment (other than USB) |
| usbenvironment                  | explore USB aspects of the ddcutil installation environment  |
| probe                           | report the capabilities string and probe the features of a single monitor |
| interrogate                     | collect maximal information for problem diagnosis            |
| chkusbmon  /dev/hiddevN &nbsp;         | used by udev rules to test if a USB device represents a monitor |

<br>
A ***feature code*** is specified as a hexadecimal  number, with or without a leading "x". For example, 
to the current brightness value:

~~~bash
$ddcutil getvcp 10
~~~

A ***feature-group*** is a named set of features.  For example:
~~~bash
$ ddcutil getvcp all
$ ddcutil getvcp color
~~~



<br>
If more than one monitor is attached, the desired monitor can be specified 
using any of several options.  The most useful are: 

***--display*** &lt;display number>  
***--bus*** &lt;i2c bus number>  

To see a list of all attached monitors and their associated identifiers:
~~~
$ ddcutil detect
~~~

These sections document **ddcutil** commands and options in detail: 

- [Command Detail](command_detail.md)
- [Command Options](options_main.md)

The  ***--help*** option provides a summary of commands and options, as does 
the man page: 

~~~bash
$ ddcutil --help
$ man 1 ddcutil
~~~

Command examples: 

- [ddcutil detect](detect_verbose_output.md)
- [ddcutil capabilities](cap_u3011_verbose_output.md)
- [ddcutil getvcp](getvcp_known_u3011_output.md)
- [ddcutil vcpinfo](vcpinfo_output.md)

