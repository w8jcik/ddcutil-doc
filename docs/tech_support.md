## Technical Support

Please direct technical support questions, bug reports, and feature requests to the [Issues Tracker](https://github.com/rockowitz/ddcutil/issues) on Github.    Use of this forum allows everyone to benefit from individual questions and ideas.

When posting questions regarding **ddcutil** configuration, please execute the following command as root, capture its output in a file, and submit the output as an attachement.

~~~
$ ddcutil interrogate
~~~

Note: ***interrogate*** is a convenience command  that wraps ***detect --verbose***, ***environment --verbose***, and (for each detected monitor) **probe**.
There is no need to submit output of the individual commands if ***interrogate*** is executed.

Before running ***interrogate*** or ***environment***, ensure that Linux command ***i2cdectect*** (typically found in package ***i2c-tools***) is installed.  ***i2cdetect*** provides an indepenent check of whether the DDC slave address (x37) is active on an I2C bus, and is used by ***interrogate*** or ***environment*** if available.

Because monitors that use USB for MCCS communication are rare, the extensive USB diagnostics are separated out into
a separate command, ***usbenvironment***, which is not packaged by ***interrogate***.

~~~
$ ddcutil usbenvironment --verbose
~~~

See also:
- [interrogate command](secondary_commands.md#interrogate)
- [environment command](secondary_commands.md#environment)
- [usbenvironment command](secondary_commands.md#usbenvironment)