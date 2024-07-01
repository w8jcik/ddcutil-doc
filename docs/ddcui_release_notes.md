## ddcui Release Notes

### Release 0.5.4

13 February 2024

As part of **ddcui** installation, file ddcui.conf is saved in directory /usr/lib/modules-load.d
to ensure that driver i2d-dev is loaded.


### Release 0.5.3

07 February 2024

Release 0.5.3 requires libddcutil from ddcutil 2.1.3 or later.

### Release 0.5.2

27 January 2024

Release 0.5.2 contains some minor cleanup. There are no significant changes.

### Release 0.5.0

16 January 2024

**ddcui** release 0.5.0 requires libddcutil.so.5.1 from ddcutil 2.1.0 or later.
Versions of ddcui prior to 0.4.0 will not build with libddcutil.so.5 from ddcutil-2.0.0 or later.

#### Display Status Detection

 - When libddcutil detects connection or disconnection of a display, ddcui 
 puts up a dialog box reporting the change and indicating that the user
 needs to redetect displays using Action->Redetect Displays.

#### Miscellaneous Changes

- If multiple "options" lines are found in a segment of configuration file ddcuirc, their contents
  are combined into a single value.

#### Building **ddcui**

- If CMakeLists.txt option ASAN is specified, cmake compiles ddcui 
using options needed for ASAN and links the executable with libasan.

### Release 0.4.2

27 September 2023

**ddcui** release 0.4.0 requires libddcutil.so.5 from ddcutil 2.0.0 or later.
Versions of ddcui prior to 0.4.0 will not build with libddcutil.so.5 from ddcutil-2.0.0 or later.


#### Passing options to ddcui and libddcutil

**ddcui** now uses a separate configuration file, $HOME/.config/ddcutil/ddcuirc, 
instead of combining the [global] and [ddcui] sections of **ddcutil** configuration
file $HOME/.config/ddcutil/ddcutilrc. 

Added options:

- ***--noconfig***, ***--disable-config-file***. Do not obtain options from 
configuration file $HOME/.config/ddcutil/ddcuirc.

- ***--libopts "options"***. Passes an option string to the shared library.  This string is appended to 
the **libddcutil** option string obtained from the **ddcui** configuration file.

Several **ddcui** command line options existed solely to pass options to the shared library.  These can now be passed using 
option ***--libopts*** or the configuration file and have been eliminated: 

- ***--nousb***
- ***--trace***
- ***--trcfunc***
- ***--trcfile***

Menu item Actions->Debug Locks reports locking the **libddcutil** as a debugging aid.


#### Logging

- ***--syslog &lt;log level&gt;*** controls what messages are written to the system log 
by **ddcui** and **libddcutil**.
Valid levels are **NEVER**, **ERROR**, **WARNING**, **NOTICE**, **INFO**, and **DEBUG**.
The default is **NOTICE**.


#### Building **ddcui**: 


CMakeLists.txt recognizes option DDCUTIL_PROJECT_DIR, which specifies the
**ddcutil** project directory in which **libddcutil** is built. Using this
option avoids having to install **libddcutil.so** in a system directory in 
order for **ddcui** to use it.

e.g. 

~~~
cmake -D DDCUTIL_PROJECT_DIR=/my/ddcutil/project/dir
~~~

<!--  options --i1, --i2 -->

### Release 0.3.0

05 August 2022

#### General User Interface Changes

- CTL-Q terminates **ddcui** (does not apply within dialog boxes)
- Errors opening /dev/i2c and /dev/usb/hiddev devices are reported using a message box
  instead of being written to the terminal. These errors typically  reflect lack of permissions (Linux error EACCESS).
- Optionally requiring the control key to be pressed when changing feature values now applies
  to all changes, not just those made using sliders. (The option is set using command line
  option ***--require-control-key*** or the UI Options dialog box.).
- The tab key was not jumping to the OK and Cancel buttons in option dialogs.

#### Feature Value Handling
- For simple NC values, do not include the SH field in validation when changing a value.
  It has been observed that there exist monitors (e.g. Dell U4320) for which the high 
  order byte of feature x60 (Input Source) can be non-zero.

#### EBUSY Errors
- As of **libddcutil** release 1.3.0, the shared library has been modified to avoid the code path in driver i2c-dev that can produce EBUSY errors.
  Options ***--force-slave-address*** and ***--disable-force-slave-address***, introduced in Release 0.2.1, are deprecated and no longer have any effect. 
  For details see [Device Busy (EBUSY) Errors](release_notes.md#release_1.3.0_ebusy) in the release notes for **ddcutil** 1.3.0.


### Release 0.2.2

22 February 2022

#### Critical Bug Fix

Fix an erroneous assert() statement that could cause a compilation failure or segfault in
the handling of features X62 (Speaker Volume). 

### Release 0.2.1

02 February 2022

#### Handling EBUSY Errors

New command line option ***--force-slave-address*** enables **ddcui** to take control of DDC communication with a display
from a conflicting program, notably display driver ddcci.
For details, see [Option: --force-slave-address](other_options.md#option-force-slave-address)
and [Device Busy Errors](release_notes.md) in the main **ddcutil** documentation.

#### CMakeLists.txt Cleanup

- The minimum required version of CMake is set to 3.10.
The CMakeLists.txt script is known to work in Ubuntu 18.04, which has CMake 13.10. 
It may work for earlier CMake versions; the cmake_minimum_required() statement 
can be edited to allow execution,
- Most messages from the script, issued using the message() statement. are defined to have message level VERBOSE.
By default, these are not shown, reducing output clutter. 
Using cmake command line option ***--log-level=verbose*** will cause them to be shown.
However, message level ***verbose*** did not exist prior to release 3.15. In that case
the messages are unconditionally issued.
- If ccache is installed, cmake command line option ***--use-cache*** controls whether
ccache is used to reduce build time.  The default is ***--use-cache=yes***.
- Better support for CMAKE_BUILD_TYPE. The default is ***-DCMAKE_BUILD_TYPE=DEBUG***.

#### Option Removed: Use Latest Feature Values

Option "Use Latest Feature Values" in the NC Feature Values dialog, and corresponding
command line option ***--use-latest-nc-values*** have been removed. This option 
told **ddcui** to use the NC values for the most recent MCCS specification, as 
opposed to the MCCS version that the monitor reports. This option was hard to 
explain and convoluted to implement.  The [User Defined Features](udf.md) facility makes it
unnecessary, since that facility can be used to define the NC values for a monitor feature.

#### Miscellaneous

- Disable selecting the Capabilities and Feature views for a monitor if it doesn't support DDC/CI,
  or the capabilities string cannot be read.
- Disable all view selection if no monitor is detected.

### Release 0.2.0

25 November 2021

While far from complete, this release should be suitable for general use.
The code base has been extensively reworked. This section summarizes user visible changes since release 0.1.2. 

ddcui 0.2.0 requires at least libddcutil.so.4.1.0 from ddcutil package 1.2.0 or later.

### Enhanced Feature Representation

- In MCCS 2.2 and 3.0, some features have both a set of special values and a continuous range.
  These include x62 (Audio Volume), x63 (Audio Balance) , x87 (Audio Tebble), X91 (Audio Bass), and x93 (Audio Balance).
  The widget for these features has both a slider/spinbox for the continuous range and a combo-box for the special values.

### Non-Continuous Feature Values

There are three interrelated mechanisms for addressing unexpected values for a simple Non-Continuous feature.

- The monitor may report a feature value that is not listed in the capabilities 
  string or the Monitor Control Command Set. In this situation the value is still listed in the 
  combo box even if another value is subsequently selected. 

- When presenting simple Non-Continuous feature, **ddcui** uses the 
  values/name table specified for the monitor's MCCS version.  Sometimes monitors 
  use feature values defined in a later version. The ***Use latest MCCS values*** 
  checkbox in the **NC Feature Values** dialog specifies that the NC value table 
  for the highest possible MCCS version be used.  This can also be specified by 
  command line option ***--force-latest-nc-value-names***.

- Supply a User Defined Feature definition.  See [User Defined Features](/udf).

### Redetect Monitors
- For situations where a display is attached or detached, 
  Menubar option ***Actions->Redetect Monitors*** can be used to redetect displays.
  The plan is for this to happen automatically in a future release.


### Miscellaneous User Interface Changes

- The ***Other Options*** dialog has been renamed to ***NC Feature Values***.
- Help screen content has been revised and extended.
- General user interface cleanup


### Desktop Integration

- Install ddcui.desktop in /usr/share/applications.
- Install icons in /usr/share/icons/hicolor. An icon is also installed in /usr/share/pixmaps
  as Gnome Classic only looks in the latter directory.
- Install ddcui.appdata.xml in /usr/share/metainfo.
- Use icon "video-display" from Oxygen theme as it already 
  exists under an open source license.
   
### Configuration File 

- Configuration file, typically $HOME/.config/ddcutil/ddcutilrc, is shared
  with **ddcutil** and **libddcutil**.  Options can specified for the **ddcui** 
  command line, and also for passing directly to the shared library. 
  See page [Configuration File](config_file.md). 


### Invocation

- Several command line options that were simply passed to **libddcutil**
  have been removed from the command line, as they can now be be specified instead
  in the ***[libddcutil*** section of configuration file **ddcutilrc**.
  These include ***--udf***, ***--no-udf***, ***--nousb***, ***--maxtries***, 
  --***sleep-multiplier***, ***--sleep-less***, ***--less-sleep***, ***--no-less-sleep***, 
  ***--dynamic-sleep-adjustment***. 
- Command line option ***--help*** does not report development related options.
  Use ***--help-development*** or ***--help-all*** to see them.


### Building **ddcui**

- qmake is no longer supported. Use cmake.


### Release 0.1.2

29 June 2020

Bug fixes: 

- The combo boxes for simple ***Non-Continuous*** values appeared empty instead of showing the current value and list of allowed values.


### Release 0.1.1

24 June 2020

#### Command Line Options 

Several command line options have been added. See [Command Line Options](ddcui_options.md)

#### User Defined (aka Custom) Feature Set

Custom feature sets enable the user to specify exactly which features are to be shown in the ***Features*** view.

Feature Slection Dialog changes: 

- Radio button ***Custom*** has been added as an alternative to ***MCCS***, ***Capabilities***, etc. for feature set selection.
- When ***Custom*** is clicked, an edit box enables entry of the feature codes to be included.  These are entered as two character hex values, with or without a leading "x' or trailing "h".  The values are separated by commas and/or blanks.

Command line option ***--custom-feature-set*** specifies the custom feature set from the command line.
For example: 
~~~
$ddcutil --custom-feature-set "10 x12, 01h"
~~~
