## Release Notes

For **ddcui** changes, see [**ddcui** Release Notes](ddcui_release_notes.md)

### 2.1.4

17 Feb 2024

Restored API function **ddca_create_display_ref()**.  See [Shared Library Changes for Release 2.1.4](c_api_214.md).

### 2.1.3

07 Feb 2024 

Release 2.1.3 fixes a bug in **libddcutil** that caused API calls to fail with status code
DDCRC_ARG (invalid argument) when passed an otherwise valid display reference for a display that doesn't support DDC/CI.
This caused **ddcui** to crash when querying information about such a display.

In addition:

- Option ***--settings*** reports build options.
- Improved determination of whether all display drivers support DRM

### 2.1.2

27 Jan 2024

Release 2.1.2 fixes a critical bug in libddcutil that caused older versions of PowerDevil to repeatedly crash and restart.

In additon:

- It fixes some low-impact bugs, and better accomodates the proprietary Nvidia video driver.
- Option ***--min-dynamic-multiplier*** is no longer marked hidden (i.e. not reported by the normal ***--help*** option).
  It sets a floor to the speed multiplier caclulated by the dynamic sleep algorithm,
  which may improve performance in some situations by reducing "jitter".
- Option ***--vstats*** additionally reports the minumum, maximum, and average successful sleep multiplier.

### 2.1.0

16 Jan 2024

#### Tuning Initialization

**ddcutil** initialization typically consumes most of the elapsed time of command execution.
For devices using DDC/CI (i.e. all but the few that communicate the with the monitor's Virtual Control Panel using USB), there are roughly speaking two phases to this process.  First, every /dev/i2c device
associated with a video display adapter is examined to determine if it has an EDID, i.e. if it is associated with a connected monitor.  After that, each of the /dev/i2c devices associated with monitors are tested to see if DDC/CI communication works for the /dev/i2c device.  This includes a check that the monitor adheres to the DDC/CI specification for reporting unsupported features; if not, **ddcutil** attempts to figure out if there is some other way the monitor reports unsupported features.

To speed up this process, the following changes have been made.

    - Option ***--ddc-check-async-min*** specifies a threshold number for performing the DDC
      related checks in parallel.  The optimal value is a tradeoff between the time savings from parellel execution and
      the overhead of splitting execution into multiple threads.  This tradeoff will vary by processor speed and the number 
      of threads available.  The default value is 3, i.e. parallelize execution if 3 or more monitors are present.
    - The ambiguously named ***--async*** option has been is deprecated and now has no effect. Previously, it turned on
      parallel execution of DDC related checks in a multi-monitor situation, no matter the number of monitors.
    - As a practical matter, once you know that your display adheres to the DDC/CI spec, there's no need to perform the DDC level
      check every time **ddcutil** is invoked.  Option ***--skip-ddc-checks*** disables the DDC level checks completely. 

<!--
      - Option ***--i2c-bus-check-async-min*** specifies a threshold value for performing the 
      the initial checks on each /dev/i2c device.  Individually, each I<sup>2</sup>C check is much faster than the DDC level check, 
      but there are many more of them to perform.  The default value for this option is 4, i.e. parallelize execution if four or 
      more /dev/i2c devices are to be examined.
--> 

#### Dynamic Sleep

  - Always set sleep multiplier to at least 1.0 for commands **setvcp** and **scs**. Addresses reports
    that aggressive optimization caused setvcp to fail.

#### Cross-Instance Locking

The cross-instance locking feature uses system call flock() to
ensure that operations on /dev/i2c devices by multiple instances of **ddcutil** or **libddcutil** do not interfere.
All instances must use option ***--enable-cross-instance-locks***.  If a conflict is encoutered, information 
is reported to the terminal.

#### Switching input source on recent LG monitors

Option ***--i2c-source-addr*** causes **setvcp** to use the specified address instead 
of the value x51 given in DDC/CI specification.  User experimentation has found that 
using an alternate source address accesses additional features on some LG monitors. 
See [Switching Input Source on LG Monitors](https://github.com/rockowitz/ddcutil/wiki/Switching-input-source-on-LG-monitors).

#### Miscellaneous Changes

- Better handling of DDC Null Message recognition and adjustments
- Dynamic sleep: make it easier to reduce time - once high didn't come down
- Command detect: ensure output of model name, serial number, and extra display descriptor 
  use only ASCII character in the range 0..127.
- Some USB-only code was not iftested out when **configure** option ***--disable-usb*** was set. (Issue 355)
- Cross-thread locking handles situtations where a display ref does not yet exist, e.g. reading EDID
- Deprecate vaguely named option ***--force***.  Replace its single use with
  option ***--permit-unknown-feature***, which applies only to command **setvcp**.
- Deprecate vaguely named option ***--ddc***. Instead use ***--ddcdata***, which more
  clearly indicates that it causes DDC data errors to be reported. 
- Option ***--version***: The amount of detail shown is now modified by options ***--brief*** and ***--vebose***.
  If ***--brief*** is specified, just the version string is shown.  If ***--verbose*** is specified, detailed 
  build information is reported.
- Option ***--help***: Show extended help on argument values only if ***--verbose*** is also specified.
- Command **detect**. EDID text fields FF (Display serial number), FC (Display Name) and FE (Unspecified Use) are 
  supposed to contain only ASCII characters, i.e. single byte characters in the range 0..127.  There have been reports
  of "garbage" output from **detect** because of characters in the range 128..255.  Any characters in the range 128..255, 
  along with control characters in the range 1..31.
- Command **detect**: Clearer messages for laptop displays.
    - Do not report "DDC communication failed"
    - Report "Is laptop display" instead of "Is eDP device" or "Is LVDS device"
- If multiple "options:" lines are found in a segment of ini file ddcutilrc, their contents are 
  combined into a single value
    
#### Building

- **configure** option ***--enable-asan*** added. Turns on ASAN related compiler options and 
  links libasan into binaries.
- **configure** option ***--enable-x11*** is deprecated and has no effect.  The X11 API is no longer used.
  Distribution packagers should eliminate requirements for X11 related libraries.  The option will
  be completely removed in a future release.



### 1.4.5

23 October 2023

Release 1.4.5 extends release 1.4.1 by backporting a **configure** command option, ***--enable-install-lib-only***, from release 2.0.0.  If this option is used, **make** will build and install only shared library **libddcutil.so.4**.  This option may be useful for Linux distributions that must install this legacy library along with the newer **libddcutil.so.5** from **ddcutil** 2.x.x, in order to support existing applications that require that earlier library.

### 2.0.0 

27 September 2023
<!-- 
add options --enable/disable-watch-dsplays

-->

Release 2.0.0 is a major update to **ddcutil**, containing significant performance improvements.
For detailed information about **libddcutil** changes, see [Shared Library Release Notes](libddcutil_release_notes.md).

Shared library **libddcutil.so.5**, built from the **ddcutil** 2.0.0 source, is not backwards compatible - 
some programs using the library may need to be recompiled. 
For detailed notes about **libddcutil** changes, see [Shared Library Release Notes](libddcutil_release_notes.md). 

#### Installation

**ddcutil** installation creates file /usr/lib/modules-load.d/ddcutil.conf
to ensure that kernel module i2c-dev is loaded at boot time (if it was not built into the kernel).

As of Release 1.4.1, **ddcutil** installation created file /usr/lib/udev/rules.d/60-ddcutil.rules, 
granting the logged on user read/write access to I<sup>2</sup>C devices associated with video 
monitors.  This file has been renamed to 60-ddcutil-i2c.rules.  New file 60-ddcutil-usb.rules
performs a similar task for USB Human Interface Devices that may provide an interface to
the monitor's Virtual Control Panel.   For unusual situations, sample files /usr/share/data/ddcutil/60-ddcutil-i2c.rules 
and  60-ddcutil-usb.rules provide alternative ways to grant permissions.

Taken together, these changes mean that **ddcutil** should just 
work "out of the box" with no special system configuration. The most signficiant exception is 
the special settings that may still be needed for the proprietary Nvidia driver.

#### Performance Improvement

<!--
The following options dramatically improve **ddcutil** startup time.  They are enabled by default.
The effect of ***--enable-displays-cache*** will be most noticable on monitors for which 
***--enable-dsa*** cannot achieve a low sleep-multiplier.
-->

**Dynamic Sleep Adjustment** 

The dynamic sleep algorithm has been completely rewritten to both dynamically increase
the sleep-multiplier factor (as needed) and decrease the sleep multiplier factor 
(insofar as possible).  Data is maintained across program executions in file 
$HOME/.cache/ddcutil/stats.
Option ***-dsa***, or one of its variants such as ***--enable-dsa*** turn it on (the default).
Option ***--disable-dsa*** turns it off.

If both ***--sleep-multiplier*** and ***--dsa*** are specified, existing statistics are
discarded and the sleep algorithm restarts calculation with the specified sleep-multiplier value.
Therefore ***--sleep-multiplier*** should generally not be used along with ***--dsa***.

<!--
**Cached Display Information**

DISABLED --f9 can enable display caching

Program startup can be slow because of the many sleeps required by the display detection process.  
This information rarely changes.  Information about the detected system configuration is optionally 
saved in file $HOME/.cache/ddcutil/displays.

Options ***--enable-displays-cache*** and ***--disable-displays-cache*** control whether
this feature is enabled.
-->

#### Performance Cache Management

There are two ways to explicitly delete the capabilities and/or dynamic sleep caches.
- Command **discard capabilities | dsa | all cache[s]**
- Option ***--dicard-cache[s] capabilities|dsa|all*** erases caches at the start of a command.


#### System Logs 

Writing to the system log has been generalized and in the process simplified.

Option ***--syslog &lt;level&gt;*** controls what is written to the system log.
Recognized levels, in order of increasing verbosity, are NEVER, ERROR, WARN, NOTICE, INFO, VERBOSE, and DEBUG.
This option replaces ***--enable-syslog***, ***--disable-syslog***, and ***--trace-to-syslog***. 
The default for **ddcutil** is WARN, and for **libddcutil** is NOTICE.

#### **environment --verbose** Changes

- Option ***--quickenv*** skips some slow tests such as
  use of program i2cdetect.
- Extended sysfs scan for ARM SOC devices to explore how those devices use /sys
- Report contents of cached files

#### Switching input source on recent LG monitors

Recent LG monitors allow for splitting the display screen between multiple input sources.
They use an undocumented protocol, instead of the (relatively) simple VCP feature X60 (Input Source),
to control how the screen is split and which input source is shown where.  Thanks to a community 
effort, particularly by user Max Thomas (Github handle [shinyquagmire23](https://github.com/shinyquagsire23)),
some of this protocol has been reverse engineered. It entails using an alternative I<sup>2</sup>C source address, 
x50 instead of x51 in the I<sup>2</sup> packet. Option ***--i2c-source-addr*** provides a way to use this alternative address in DDC communication.
For further details, see [wiki page](https://github.com/rockowitz/ddcutil/wiki/Switching-input-source-on-LG-monitors).
The extended discussion summarized on the wiki page occurred on [ddcutil issue 100: LG 29UM69G fails switching input](https://github.com/rockowitz/ddcutil/issues/100).

#### **ddcutil detect** Changes

- Option ***--verbose*** reports the current dynamic sleep muliplier. 
- Improve determination of non-standard ways that unsupported VCP features are reported, 
ane report that output may be incorrect if detection of invalid features is unreliable.
- Improved identification of "phantom" displays when a monitor is capable of DisplayPort Multi-Stream Transport passthrough.
- Warn that output may be inaccurate if the monitor appears to be in a sleep mode.

#### Miscellanous Changes

- Option ***--sleep-multiplier 0*** is now allowed. Some DisplayPort monitors
  have been observed to work with this value.
- The tests durng display detection to check for misuse of the DDC Null Message
  or an all zero getvcp response to indicate unsupported features have been made more robust.
- Option ***--noconfig***: Do not process the configuration file.
- Option ***--trcfrom &lt;function name&gt;***: Trace the specified function and its called functions.
  (This option reports only functions for which tracing has been enabled.)
- Option ***--hh***. The number of command line options has become huge. Many 
  are development related.  Options not of interest to general users are now
  hidden.  Option ***--hh*** exposes them, and implies option ***--help***.
- Option ***--verbose***.  If specified on the command line, the options obtained
  from the configuration file are reported.
- Detailed statistics are now maintained on a per-display instead of per-thread basis.
  Option ***--vstats*** reports detailed statistics, in particular per-display 
  statistics.  ***--vstats*** takes the same arguments as ***--stats***. 
- Option ***--help***. Document **ELAPSED** as a recognized statistics class
- Option ***--help*** reports option ***--force*** as deprecated.
- Reading the EDID when using USB to communicate with a monitor's virtual control
  panel can fail.  Try using a fallback mechanisms to read the EDID for any Eizo displays, 
  not just those with a specific product id.
- Existing cached capabilities are not erased when ddcutil is called with capabilities
  caching disabled.
- Options ***--ignore-hiddev*** and ***--ignore-usb-pid-vid*** cause **ddcutil** initialization
  to ignore USB Human Interface devices that could possibly provide standards-conformant USB access to the 
  monitor's Video Control Panel. **ddcutil** already ignores HID devices that are declared as 
  keyboards or mice, as probing such devices can cause system lockups.  The options are provided because 
  it is conceivable that other such devices exist.  
- Improve reporting of /dev/hiddev open failures, to avoid confusion resulting from this 
  typically benign error.
- Fix segfaults in cacheing due to $HOME not set.
- Fix segfault in shared library termination.
- **ddcui** now uses a separate configuration file, $HOME/.config/ddcutil/ddcuirc.
- If X11 is the display manager, terminate immediately if the monitor is in a DPMS sleep mode.
  For Wayland, the best that can be done at present is to detect if an individual monitor is in a DPMS sleep mode.
  In that case, or the case of no display manager, DPMS status is determined during display detection.



### Building

- **configure** options **-enable-syslog**/**--disable-syslog** have been removed.
  Instead, use command line option **--syslog never** at runtime to disable all writes to the system log.
- Shared library libkmod is no longer required.
- Shared library libjansson is now required. Its development package (libjansson-dev on Debian based systems) must be installed.
- If **congfigure** option ***--enable-install-lib-only*** is specified, command **make install** installs only
  the shared library

### Development Facilities

- Added termporary use options --f7, --f8, --i2, --s1, --s2, --s3, --s4, --fl1, --fl2.
  The current meaning of these options is reported by option ***--settings***. 
- Added utility command C1 for temporary use during development.
- Added option ***--enable-mock-data***




### 1.4.1

16 January 2023

Release 1.4.1 fixes a critical bug in release 1.4.0 affecting both command line program **ddcutil** and shared library **libddcutil**.
The default sleep-multiplier value was 0 instead of 1.  As a result,
commands that did not have an explcit ***--sleep-multiplier*** option would fail.  All users of release 1.4.0 should upgrade.
Thank you github users copysiper (Oleg Tsvetkov) and fionnb for your help in quickly identifying and resolving this bug.

### 1.4.0

13 January 2023

### Nvidia Proprietary Driver Workaround

Release 1.3.0 simplified coding by exclusively using the ioctl() interface in driver i2c-dev as opposed to the write()/read() file io interface.
This had the additional benefit of making option ***--force-slave-address*** irrelevant.
Unfortunately, this change exposed an error in how the proprietary Nvidia driver is coded.  Because of this error, depending on the version of header file i2c.h used when the driver is built, all reads and writes using ioctl() fail.

Therefore, the ability to use the write()/read() intead of ioctl() for writing and reading has been restored. By default, the ioctl interface is used for all drivers.  The initial IO mechanism used can be forced by new options ***--use-file-io*** and ***--use-ioctl-io***, but these options are really only for testing purposes.  **ddcutil** will revert to using the file io interface if the Nvidia error is detected using the ioctl interface.  There should be no benefit to using the file io interface with other than the Nvidia driver.
Note, however, that when using the file io interface, option ***--force-slave-address*** is once again relevant for handling EBUSY errors.
 
### I2C Device Permissions Simplified

Until now, the recommended way give-non root users access to the I2C devices is as outlined on the existing [I2C Device Permissions](http://www.ddcutil.com/i2c_permissions/) page. In short, all /dev/i2c devices are assigned to group i2c, and have read/write access to those devices. This happens automatically in ddcutil packages for Debian and derived distributions such as Ubuntu, because package ddcutil depends on package i2c-tools. All that remains is for ddcutil users to be added to group i2c.

Release 1.4.0 simplifies permissions by adding a udev rule that uses the uaccess tag to give the logged on user read/write access to all /dev/i2c devices associated with video adapters. 
(See [issue 275](https://github.com/rockowitz/ddcutil/issues/275)). Installing package ddcutil installs file /usr/lib/udev/rules.d/60-ddcutil.rules (or /usr/local/lib/udev/rules.d/60-ddcutil.rules). 
If building from source without actually installing ddcutil, you can copy file data/usr/lib/udev/rules.d/60-ddcutil.rules to directory /etc/udev/rules.d.
Thank you to github user digitaltrails (Michael Hamilton) for suggesting this change.

### libddcutil Changes
- The ABI is unchanged from release 1.3.0.  The full shared library name is libddcutil.4.3.0.
- **ddca_set_default_sleep_multiplier_value()**, **ddca_set_sleep_multiplier_value()**: Accept 0 as a valid argument.
- **ddca_enable_force_slave_address()**.  This function was made a NO-OP in release 1.3.0.  It is once again has effect. 

For details on **libddcutil** changes, see [Shared Library Changes for Release 1.4.0](c_api_140.md).

### Building ddcutil

- New configure options: ***--enable-syslog/--disable-syslog*** control whether messages are 
written to the system log, which can pose problems if executing in a container.  The default is 
***--enable-syslog***.
- Compiler option ***-WError*** is only used when building a development version of ddcutil, e.g. 1.4.0-dev. 
If ***-WError*** is specified, compilation can fail on a system other than the development system, 
having different compiler warnings enabled.
- configure.ac: Only invoke deprecated macro AC_PROG_CC_99 if autoconf version < 2.70

### Suppress sleeps

Option ***--sleep-multiplier*** and API functions **ddca_set_default_sleep_multiplier_value()** and **ddca_set_sleep_multplier_value()** now accept 0 as an argument, which disables sleeps.  This is intended for calling programs that wish to control sleep time themselves.

### Multiple monitors having the same EDID

It is rare, but possible, to have multiple monitors with identical EDIDs. For example, the ASUS PG329Q has neither a character or a binay serial number in its EDID, and a user had a pair of monitors built in the same week, so the EDIDs were identical. 

- If multiple monitors with identical EDIDs are used with the nvidia proprietary driver, 
the DRM connector name cannot be reliably determined.  Command **detect** issues a warning in this situation.

### Miscellaneous

- Command **environment --verbose**: Report the build options in effect.
- The **ddcutil** command parser reports an error if a display selection option (e.g. ***--bus***,  ***--model***) is given on a command to which it does not apply.
- Handle /dev/i2c device names with a double hyphen, e.g. /dev/i2c--3. 
- libddcutil: Better handling of configuration file errors, do not abort. See [github issue #289](https://www.github.com/rockowitz/issues/289). 
- Write additional error and information messages to the system log
- Command **detect**: Eliminate message "Is DDC/CI enabled in the monitor's on-screen display?"  It's rarely the problem.
- Command **detect --verbose**: For EDID version 1.4, correct the interpretation of the digital display type bits, e.g. "RGB 4:4:4".
- Fix miscellaneous segfaults


### 1.3.2

04 September 2022

**ddcutil** release 1.3.2 modifies tarball creation to eliminate garbage and otherwise unneeded files.
Users of release 1.3.0 need not upgrade.

<a name="release_notes_1.3.0"></a>
### 1.3.0

23 July 2022

### Brickable Monitors

There have been a handful of reports of monitors for which setting a feature value, most commonly
in the manufacturer reserved range, puts the monitor into some difficult to recover state.
A warning is issued at startup for those monitors.  Currently the only monitor in the warning list is 
Xaomi model "Mi Monitor". 

<a name="release_1.3.0_ebusy"></a>

### Device Busy (EBUSY) Errors

Driver i2c-dev provides both a low level ioctl() interface and a higher level read()/write() 
interface for I2C communication.  **ddcutil** now uses the ioctl() interface (almost) exclusively. 
This eliminates the possbility of EBUSY errors from driver i2c-dev, which occur only when the read()/write() interface
is used. As a result, option ***-force-slave-address*** no longer has any effect.
In principle, EBUSY errors are still possible from within individual video drivers invoked by i2c-dev, but this has never been observed.

### Better Report User Configuration Issues

- More user friendly messages at startup regarding /dev/i2c buses that cannont
  be opened.  If the problem is inadequate permissions (EACCES), the user is 
  directed to [the web site](https://www.ddcutil.com/permissions).
- Report at startup if required device driver i2c-dev is not built into the kernel
  and has not been loaded.

### Command **detect**

- Option ***--verbose*** produces addtional information: 
   - The product code is reported in hex as well as decimal
   - The EDID source field is set to **I2C** in the normal case where the EDID
     is read directly from slave address X50.  Alternative values include 
     **USB**, **X11**, and **SYSFS**.

### Commands **getvcp**, **vcpinfo**

- Allow specification of multiple feature codes, for example 
    ***ddcutil getvcp 10 12*** , ***ddcutil vcpinfo 14 16 18 1a***

### Command **environment**
   - Scanning of /sys by option ***--verbose*** has been improved.

### Better handle malformed EDIDs
  - Trailing blanks on model and serial number are stripped.  This affects 
    commands **detect --terse**, **loadvcp** and **dumpvcp**, and also the 
    file names of user defined features.

### Option ***--stats***: 
  - I2C ioctl() calls for reading and writing are now reported as type IE_IOCTL_WRITE
    and IE_IOCTL_READ rather than IE_OTHER.
  - IE_WRITE_READ stats are no longer reported, as they are redundant.

### Performance Options

- Option ***--dsa***/***--dynamic-sleep-adjustment***: The Dynamic Sleep Adjustment algorithm was rewritten to 
  more sensibly increment sleep times after before each retry.
- Sleeps immediately after opening a /dev/i2c device and after completion of a
  read operation are completely eliminated. The sleep-suppression related 
  uptions, ***--sleep-less***, ***--less-sleep, ***--enable-sleep-suppression***,
  and ***--disable-sleep-suppression*** no longer have any effect.

### Miscellaneous Bug Fixes

- The sleep multiplier value was not respected for new API threads.
- User Defined Features: Keyword **NC** set the incorrect flag in a feature
  descriptor.
- Fixed a segfault in **ddcutil** initialization because of unexpected
  contents in sysfs.
- Double count I2C writes in stats. 

### libddcutil
- Fixed a segfault that occurred at shared library startup.  The fault was in a 
  trace message for function ddc_start_watch_displays() which watches for
  displays that are added or removed.
  - Deprecated API functions have no effect:
    - **ddca_enable_force_slave_address()**, **ddca_is_force_slave_address_enabled()**

For details on **libddcutil** changes, see [Shared Library Changes for Release 1.3.0](c_api_130.md).


### 1.2.2

28 January 2022

### Device Busy (EBUSY) Errors

This release improves the handling of and messages regarding DDC communication failures
with errno EBUSY. In particular, this error occurs when driver ddcci is loaded.

- Command **detect**: If DDC communication fails with error EBUSY, report the
  display as "Busy" instead of "Invalid" and suggest use of option ***--force-slave-address***.
- Command **environment**: If driver ddcci is loaded, suggest use of option ***--force-slave-address***.
- Messages regarding EBUSY errors are always written to the system log.
- Added API functions **ddca_enable_force_slave_address()**, **ddca_is_force_slave_address_enabled()**. 

### Building and Porting

- When building **ddcutil**, allow for linking a static library if **configure**
option ***--enable-static*** is set. By default, static libraries are not built,
since Linux distributions generally frown on their inclusion in packages.
- Calls to function **__assert_fail()** have been replaced with **exit()** in
traced assertions.  **__assert_fail** is part of the Linux implementation of **assert()**, 
but is not in the C specification.
This change simplifies porting of **ddcutil** to environments lacking this function. 
- Avoid compilation warnings when assertions are disabled (NDEBUG is defined).
<!--
- Disable proof of concept code that spawns a separate thread in **libdcutil**
that watches for display changes.
-->

### Miscellaneous Changes

- Command **detect** reports additional EDID detail for option ***verbose***.
  This information, including EDID version and color related detail, has no effect on **ddcutil** execution but may be of general interest to some users.

### Fixes

- Only write messages to the system log regarding **ddcutil** starting and termination if option ***--syslog*** is specified.

### **libddcutil** Changes

For details on **libddcutil** changes on releases 1.2.0/1.2.1/1.2.2, see [Shared Library Changes for Release 1.2.0](c_api_120.md).

### 1.2.1

02 December 2021

### Option ***-settings*** 

Until now, details of the current configuration and option settings have been shown
at the start of any command when ***--verbose*** is in effect. This information 
has become increasing large and distracting. Instead, this information is now
reported only when option ***--settings*** is specified. 

### Tracing

- Option ***--syslog*** sends trace and debug messages to the system log in addition 
  to the trace location.
- Option ***--wall-timestamp--*** or ***--wts*** preface trace and debug messages with
  the current wall time. 

### Fixes

- Build would fail when **configure** option ***---enable x11=no*** was specified.
- API functions **ddca_open_display()** and **ddca_open_display2()** now always return 
  **DDCRC_ALREADY_OPEN** if the display is already open in the current thread.
  Previously an assert failure would occur under certain circumstances.
- Numerous memory leaks in **libddcutil** have been plugged.

### 1.2.0

05 October 2021

Most visible changes in release 1.2.0 affect only shared library **libddcutil**.
Users of the command line program **ddcutil** do not need to update. 

### General Changes

- Option ***--enable-capabilities-cache*** is now the default.
- Command **detect --verbose** now reports the raw EDID.
- Major events are written to the system log.  These include starting, stopping, and severe internal errors.
- Fix for github issue [#178: Error detecting i2c-dev kernel module](https://github.com/rockowitz/ddcutil/issues/178).  The checks now use library libkmod 
  (which is the basis for commands modinfo, lsmod, etc.).

### **environment** command
- If programs **get-edid** and **parse-edid** are installed, command **ddcutil environment --verbose** uses them
for an addiional EDID check
- Extend sysfs diagnostics in **ddcutil environment --verbose** to better analyze problems in DisplayPort 
  Multi-Stream Transport, in which a display appears as 2 different /dev/i2c devices.
- **ddcutil environment --verbose** now includes all the information of **ddcutil detect --verbose**.
  There is no need to submit the latter along with the former in issue reports.

#### **libddcutil** Changes

See [Shared Library Changes for Release 1.2.0](c_api_120.md)


### 1.1.0

05 April 2021


### Docking station connected displays

At long last, Linux kernel release 5.10 implemented DDC communication 
over DisplayPort Multi-Stream Transport.  Most commonly, this is seen with displays 
connected to docking stations.  For several years now, all docking
station connected displays, even those using HDMI or DVI connections, internally
rely on a DisplayPort MST connection between the computer and the 
docking station. 

The kernel changes were made in the Direct Rendering Mode (DRM)
subsytem.  The Intel i915 and AMD amdgpu drivers, which are implemented as 
part of DRM, have been tested and (mostly) work. Other drivers  part of the kernel
DRM component, e.g. nouveau, should also work but have not been tested.
Nvidia's proprietary driver uses it own proprietary DRM implementation and still 
does not support DDC communication over MST.  

Unfortunately there are problems with the DRM code, which ddcutil can only sometimes work around.

Sometimes the display's EDID cannot be read on I2C slave address x50.  As a result, 
ddcutil does not report the existence of a display connected to the /dev/i2c device.
***ddcutil*** attempts to work around this by trying to read the EDID multiple times with both 128 and 256 block
sizes. Depending on the monitor, usually one or the other will work, but sometimes neither do.  
Option ***--edid-read-size***, which was introduced in release 1.0.1, can force the block size
to 128 or 256.

This problem can be unrecoverable when the computer suspends and resumes, requiring reboot.

On the other hand, the same monitor may appear as two different /dev/i2c devices, only one of which 
supports DDC.  Curiously EDID read failures are associated with the /dev/i2c device that supports DDC. 
**ddcutil** attempts to detect which /dev/i2c device should be used for monitor
communication, and the **detect** command reports the other as a "Phantom Display" instead of an
"Invalid Display".  (Thank you laur89 for the wonderfully apt term.)

My sincere thanks to github users laur89 and boscard for their considerable help in diagnosing this situation and testing (partial) solutions.

### Configuration file **ddcutilrc**

**ddcutil**'s command options have proliferated, often to address problems specific to a 
small number of users. To simplify their use, they can now be specified in a configuration file.

For details, see [Configuration File](config_file.md)


### Persistent capabilities

**capabilities** is the most expensive **ddcutil** command in 
elapsed time.  It is also the most prone to failure on marginal I2C host/monitor 
connections, due the large number of I2C requests involved.

On the other hand, the capabilities string is constant for a monitor model.
**ddcutil** now caches capaiblities strings, significantly speeding up execution of the **capabilities** command.

This feature is controlled by options ***--enable-capabilities-cache*** and ***--disable-capabilities-cache***.
The default is  ***--enable-capabilities-cache***.  ***--disable-capabilities-cache*** may be needed as there are some edge cases not yet addressed

The cache is located in file **ddcutil/capabilities** of the XDG cache directory. 
Typically this is $HOME/.cache/ddcutil/capabilities.  This file can safely be erased if the 
stored capabilities string should become corrupted in some way. 

### Miscellaneous

- Warn the user at **ddcutil** startup if module **i2c-dev** is neither loaded nor built into the kernel.
This should help new users properly configure their systems.  
- More detailed output on the ***detect*** command, particularly with option ***--verbose***.  
- ***environment --verbose*** has more extensive diagnostics, particulary of /sys 
- Avoid corrupting the state of AMD Navi4 gpus, e.g. RX 6000 series, by not examining SMU I2C buses on the 
video adapter.

### Building **ddcutil**

***configure*** option ***--disable-usb*** now implies ***--disable-udev***. 
This addresses a build failure reported by Amon Sha.


### 1.0.1

08 February 2021

This bug fix release addresses the situation wherein a display connected through a docking station was not detecteed 
when the i915 video driver is used.  (I2C support for docking station connected displays, and more generally displays using 
DisplayPort Multi Stream Transport. was recently implemented for i915 and amdgpu drivers in Kernel 5.10. Initial **ddcutil**
code for this situation did not adequately work around problems in the driver implementation,  with the result that the EDID
was not read in certain contexts.

### Option ***--edid-read-size***

Command line option ***--edid-read-size***, introduced in release 1.0.0, is no longer needed.  **ddcutil** not attempts
both 128 and 256 byte reads to fetch the EDID.

### 1.0.0

03 February 2021

### User Defined Features

***ddcutil***, for some time now, has implementated user supplied feature definitions. This facility is now formally available. 
A feature definition file for a monitor model specifies feature definitions that add to or override those in the MCCS spec. It can also optionally force the MCCS version of the monitor. A user supplied feature definition can, for example, specify a monitor's actual values for a Non-Continuous feature such as x60 (Input Source), or define a feature in the manufacturer supplied range xE0..XFF. The facility is enabled by option ***--udf***. The default is currently ***--noudf***.  See page [User Defined Features](udf.md) for details.

### USB

By default, ***ddcutil*** no longer checks for monitors that use USB to communicate with the 
monitor's ***Virtual Control Panel***.  Such monitors are rare, and the error messages that occur when  
permissions have not been granted on /dev/usb/hiddev devices can be confusuing.  USB display detection 
can be enabled using command line option ***--enable-usb***.  The default is ***--disable-usb***. 

#### Command **detect** 

- If no displays are found, add a message suggesting ***ddcutil environment***  
- Always show the product code and binary serial number  
- Additional /sys information for ***--verbose*** output  
- Revised logic to better handle some marginal monitor/driver cases

### Command **capabilities**

- The **capabilities** command now issues detailed error messages for malformed capabilities strings, and 
attempts to continue if recovery from the parsing error can produce useful information. 
 
- Fixed instances of assert failures when parsing a capabilities string with unmatched parentheses or the 
in which the model name is unparenthesized.

- In the "Commands" segment, the id values are referred to as "operation codes" for consistency with 
DDC/CI documentaion. Capitalization in command output is more consistent.

### Command **getvcp**

- If **getvcp** does not know how to format a value, it now includes the mh, ml, sh, and sl byte values in the "UNABLE TO FORMAT OUTPUT" message.

- Implement special handling for features x62 (audio volume) x87 (audio treble), x91 (audio bass) and x93 (audio balance).
In MCCS 3.0 and 2.2, these features have special reserved values along with a range of continuous values.

- Fix the cause of an assert failure when showing the value of feature x93 (audio balance). 


### Command **setvcp**

- The parsing of **setvcp** arguments has been rewritten to correctly allow for more than 1 relative values. 
- A command such as "setvcp 10 +15" was incorrectly being interpreted as indicating a 
relative feature value.  The "+" or "-" in relative values must be surrounded by spaces.

<!--
The syntax for specifying relative values has been relaxed.  Spaces are no longer required around "+" or "-".
For example, the following all specify relative values: 
~~~
$ ddcutil setvcp 10 + 5
$ ddcutil setvcp 10-5
$ ddcutil setvcp 10 +15
~~~

A consequence of this change is that a plus sign in from of a value is no longer indicates a positive number. 
That is, in the final example above, ***setvcp*** increases the value of feature 10 by 15.  It does not set 
the value of feature 10 to 15.
-->


### Command **dumpvcp**

- If no file name is specified on the command line, **dumpvcp** now writes its output to a newly created file in directory $HOME/.local/share/ddcutil. 

- Documentation has been changed to conform to code behavior. 

- Fixed the cause of the user name being null in the generated portion of the file name. 

An example of a generated file name is: 

DELL_P2411H-F8NDP11G119U-20201206-091307.vcp

where:  
- ***DELL_P2411H*** is the model name reported by the EDID  
- ***F8NDP11G119U*** is the ASCII "serial number" in the EDID  
- ***20201206-091307*** is a timestamp of when the file was created



### Command **watch**

- Better handle fatal errors instead of continuing in an infinite loop

### Command **environment**

- If option ***--verbose*** is specified, the /sys file system is probed far more extensively. This change also 
applies to command ***interrogate***, which incorporates ***environment --verbose***.

### Option ***--no-x52-fifo***

Features x02 (new control value) and feature x52 (active control) are used together to report
feature values changed using the On Screen Display (OSD). 

In MCCS 2.0 and 2.1, if feature x02 has the value x02 (changed controls exist) feature x52 is read once to get the id of a feature that has changed.
The changed feature can be read in the usual way.
Feature x02 is then read again to see if there are more features that have changed.

In MCCS 3.0 and 2.2, feature X52 is supposed to be a FIFO of the ids of
changed features.  It is intended to be read until value x00 is returned, indicating
no more features.
    
It has been observed that at least for some v2.2 monitors (HP Z22i), feature x52 never
returns x00, just always returns the same feature id. This option forces the
**watch** command to treat all monitors as having the VCP 2.0/2.1 behavior.



### Option ***--mccs &lt;vcp version>***

Option ***--mccs*** now applies to commands **getvcp**, **setvcp**, and **dumpvcp** in addition to **vcpinfo**.
See documentation of the individual commands for details.

### Option ***--per-thread-stats***

Option ***---per-thread-stats*** causes ***--stats*** output to include per thread statistics. 

By default, the ***--stats*** option does not display per thread statistics. These statistics 
are generally meaninful only in the shared library context, and their display is simply TMI<sup>TM</sup>.

<!--
<sup>&#x2122;</sup> <sup>. 

TO CONSIDER: --per-thread-stats as an alternative to --stats
--> 

### Option ***--enable-sleep-less***

Option ***--sleep-less***, now also known as ***--enable-sleep-less*** is now the default.  This eliminates some sleeps after reading DDC packets,
and does not appear to affect stability.  Option ***--disable-sleep-less*** restores the prior behavior. 

### Option ***--edid-read-size  &lt;128|256>***

This option is a work-around for a problem seen with the i915 driver and docking stations. The EDID is read successfully every other time.
Older EDIDs are 128 bytes in length, newer ones are 256 bytes. 
Strangely forcing ***ddcutil*** to read 128 bytes resolves this problem.

<!--
### Desktop Integration

If a repository containing **ddcutil** is enabled, **ddcutil** is installable by the **KDE Discover** application explorer (plasma-discover)
and **GNOME Software** (gnome-software) app store programs.

To provide for this, **ddcutil** installs the following files:  
- /usr/share/applications/ddcutil.desktop  
- Icon files are installed in the /usr/share/icons/hicolor tree  
- For backward compatibility, icon file /usr/share/pixmaps/ddcutil.png is installed.  
- /usr/share/metainfo/ddcutil.appdata.xml
-->
<!--
TODO: Notes on problems with app stores
-->


### Miscellaneous Changes and Bug Fixes 

- Fix trace message corruption when both ***--thread-id*** (***--tid***) and ***--timestamp*** (***--ts***) are both specified.   
- If any tracing is enabled, report the **ddcutil** starting and ending clock times.  
- More functions can be individually traced using ***--trcfunc***.  
- Check during command parsing that functions specified by --trcfunc are in fact individually traceable.  
- Option ***--nodetect*** is eliminated. It was reduncant since using the ***--bus*** 
option forces display detection to be skipped, and this is the only context in which 
option ***--nodetect*** was recognized.   
- Fix the recording of the minimum and maxium maxtries values that occur in each thread.  
- Correct ***--help*** output to indiate that "0" or "." in a ***--maxtries*** value string 
leaves the value of a try type unchanged.  This was incorrectly documented as "." or "".
For example, "--maxtries "., 15, 0""  changes only the write-read maxtries value. 
- At startup, check that module i2c-dev is either loaded or built in.   
- An unsupported feature is no longer regarded as a DDC data error (reported by option ***--ddc***).  
- Reduce the number of error messages written to **stderr** instead of **stdout**.  
- Bring the **README.md** file up to date.
- More definitively identify LVDS laptop displays. 


### Building **ddcutil**

Some ***configure*** options have been added, primarily in support of an as yet incomplete FreeBSD implementation. 
(Development is suspended until FreeBSD's video drivers expose the I2C bus.)

| Option             | Default | Description 
|--------------------|---------|-----------------
| --enable-asan      | no      | Build for execution with the Google Address Space Sanitizer.
| --enable-envcmds   | yes     | Controls whether commands **environment** and **usbenvironment** are included.
| --enable-udev      | yes     | Controls whether udev is used. 
| --enable-targetbsd | no      | Build for FreeBSD. 

<br>
Notes:  
- Option ***--disable-envcmds*** forces ***--disable-drm***, and ***--disable-x11***   
- Option ***--enable-targetbsd*** forces ***--disable-envcmds***, ***--disable-udev***, and ***--disable-usb***

Option-***-with-adl-headers*** has been removed.

**configure** help messages for options that are not intended for public use are now marked "(Developer only)".

Use of the **.gitignore** file has been simplified.  It now identifies only files that should be ignored by 
any clone of the github ddcutil repository.  Users should now list files specific to their copy of the repository
in the file **.git/info/exclude**, which is not tracked. 



#### Shared Library

The SONAME for **libddcutil** is now libddcutil.so.4.
For details on changes, see [Shared Library Changes for Release 1.0.0](c_api_100.md).

### Acknowledgements 

Thank you to the many users who have not not only submitted problem reports but
have helped remotely diagnose **ddcutil** issues.  In particular: 

- github users [suuuehgi](https://github.com/suuuehgi) and [laurisriple](https://github.com/lauritsriple) provided extensive help
diagnosing problems with the i915 driver and docking stations.
- [PJ Aitken](https://github.com/pjaitken) has put considerable effor into
reverse enginering Picture-by-Picture mode on a Phillips 499P9.
- github user [parkerlreed](https://github.com/parkerlreed) helped diagnose deviations from the specification of feature x60
on an Acer ED320QR. 
 
And also a shout out to [Andrey Rahmatullin](https://wiki.debian.org/AndreyRahmatullin) who sponsors **ddcutil** on Debian and
has help bring its packaging up to Debian standards.

### 0.9.9

13 June 2020

<!--
### User Defined Features

User supplied feature definitions are now supported.  For details see [User Defined Features](udf.md).
-->
#### Execution Statistics

Additional per-thread statistics are shown when ***--verbose*** is specified along with ***--stats***. 

Statistics output has become voluminous.  New  group ***ELAPSED*** (alternatively ***TIME***) shows only the elapsed execution time.
For example
~~~
$ ddcutil detect --stats elapsed
~~~

Statistics now seperately reports multi-part read and multi-part write tries.  This reflects how statistics are implementated, allowing for ***Table*** features.
As a practival matter, no monitor has ever been seen that implements features of type ***Table***, so the 
multi-part write statistics will always be zero.

#### Performance Related Command Options

Option: ***--less-sleep***, ***--sleep-less***

Eliminates many of the sleeps between the time that ***ddcutil*** receives a response packet from the display and then sends the next request packet.
Depending on monitor, this can markedly improve performance, at the cost of more retries owing to increased DDC communication errors. 
Option ***--sleep-multipler*** still applies to the remaining sleeps.  


##### Less Useful Performace Related Command Optons

The remaining new performance related options seemed like good ideas, but do not appear to 
significantly reduce execution time.  They are included for user experimentation and feedback.
They may be removed in future releases.

Option: ***--lazy-sleep***

Causes some sleep events required by the DDC/CI protocol to occur in deferred mode. 
Instead of immediately sleeping after DDC IO events that require an interval before the next request 
is made of the display, the required sleep time is
added to the current realtime value and saved in the associated
display reference.  Before the next DDC IO message to the display, the value
in the display reference is checked, and if greater than the
now current real time, a sleep for the remaining time occurs.

Unfortunately, this option appears to have only a neglible effect on execution time. 
Execution statistics show an approximately 1 ms reduction in sleep time, e.g. 50 to 49 miliiseconds.
This negligable effect reinforces the fact that most of **ddcutil**'s elapsed time is spent in DDC mandated delays
and waiting for DDC IO to complete.  '

Option: ***--dynamic-sleep-adjustment***, (alternative name ***--dsa***)

If retry is required, dynamically increases the delay time before subsequent retries, 
up to a maximum.  Does not appear to have an appreciable effect on retry success, but it 
may be a useful alternative to option ***--sleep-multiplier***. 


#### Other Options

Option: ***--timeout-i2c-io***

Causes (most) I2C read and write calls to eventually time out if they do not return.
This has been implemented as a POSSIBLE way to address occasional reports of **ddcutil** locking up, with subsequent
 **ddcutiil** calls blocking behind it.   This may become the default in future releases.

#### Support for **fglrx** removed

***ddcutil*** no longer supports AMD's old proprietary display driver ***fglrx***.  Command line option ***--adl*** is no longer recognized.
***configure*** option ***--with-adl*** has been removed.




#### Miscellaneous

- Command **ddcutil environment** should have output the message "Group i2c does not exist" if 
group i2c was not found.  The critical word "not" was missing.  Thank you Michaël Defferrard.
 
- The display identifier cross-reference table output by command **ddcutil environment --verbose**
has been modifed to handle multiple displays with an identical EDID. Certain LG Ultrawide
monitors contain neither an ASCII nor binary serial number in their EDID.  Consequently, all monitors of the same model have the same EDID. 

- Display detection now reads the EDID a byte at a time, similarly to 
command **get-edid**. This is occasionaly more 
reliable than the standard multi-byte read code.  Thank you "InconsolableCellist" 
for pointing out this difference between **ddcutil** and **get-edid**. 

- I2C slave address (DDC) is once again checked during display detection. 
This had been removed in release 0.9.3 because it caused lockups on 
Dell XPS 13 laptops.  To address this situation, slave address x37 is not 
checked for laptop displays, which never support DDC. 
Command **ddcutil detect --verbose** reports the result of the x37 test.
This is presently purely for informational purposes.

- Correctly handle multiple relative values in a single setvcp string, e.g. 
~~~
$ddcutil setvcp 10 + 5 12 - 3
~~~ 
This frankly not very useful corner case had not been taken into account 
when relative ***setvcp*** values were implemented.  Well, now it is.

- ***ddcutil*** issues an error message instead of aborting if an utterly nonsensical
***--sleep-multiplier*** value greater than 100 is specified, e.g.
~~~
$ ddcutil detect --sleep-multiplier 200      # WRONG
~~~
- Verification is always disabled if setting VCP feature xDF (Power Mode) to 5.
This is a write-only value that turns off the display.
Verification can never succeed. 

- Correct the spelling of "Finnish" in the list of On Screen Display languages for 
feature xCC.  Thank you Simon Alling.

#### Tracing

- Trace group ***RETRY*** has been added. This subset of trace group ***DDCIO*** reports only 
retry activity. 

- In a handful of unusual situations, trace messages are issued unconditionally, 
irrespective of the trace settings in effect.  This is to highlight some rare situations, such as 
recovery from multiple DDC Null Messages.
~~~
$ ddcutil getvcp 10
(ddc_write_read_with_retry) ddc_write_read() succeeded after 3 Null Messages
~~~
If you encounter such a message, please report the circumstances in which it occurred.


### Building ddcutil

- As noted above, ***ddcutil*** no longer supports AMD's old proprietary video driver ***fglrx***.
***configure*** option ***--with-adl*** no longer exists.

- Reflecting changes to how the PCI and USB hardware ID tables are packaged,  **ddcutil** now prioritizes **pci.ids** and **usb.ids**
files in directory /usr/share/misc over those in /usr/share/hwdata.
Packaging requires packages pci.ids, usb.ids, and usbutils, not hwdata.
Corresponding changes have been made to configuration file **configure.ac**.
Thank you Pino Toscano.

- Use of include file &lt;sys/cdefs.h\> has been eliminated. This include file does not exist on Alpine Linux. 


#### Shared Library

The SONAME for **libddcutil** is now libddcutil.so.3.
For details see [Shared Library Changes for Release 0.9.9](c_api_99.md).





### 0.9.8

30 November 2019

#### Command line option **--sleep-multiplier**

The DDC/CI specification dictates that the host computer wait 40-200 ms (depending on operation) between sending a command to the monitor and reading the response.
As a result, ***ddcutil*** spends approximately 90% of its elapsed time sleeping.  Many monitors respond properly with much shorter waits.
On the other hand, there are some monitors that require longer waits to avoid DDC/CI errors.
Option ***--sleep-multiplier*** applies a floating point multiplication factor to the DDC/CI specified sleep times.
For example,  
~~~
--sleep-multiplier .5 
~~~
causes 40 ms waits to become 20 ms, and 
~~~
--sleep-multiplier 4
~~~
causes 40 ms waits to beome 160 ms. 

Note that ***ddcutil*** may automatically increase wait times when peforming retries.  Option ***--sleep-multiplier*** applies to the inital wait time.

Option ***--sleep-multiplier*** can significantly speed up **ddcutil** execution - some monitors have been seen to operate properly with a sleep-multiplier as low as .1, 

Undocumented option ***-y*** for adjusting wait times is removed.

#### Command line options **--thread-id** and **--timestamp**

If option ***--thread-id*** (alternatively ***--tid***) is specified, trace messages are prefixed with the thread id number, e.g.
~~~
[  27044](ddc_get_nontable_vcp_value    ) dh=Display_Handle[i2c: fd=3, busno=5], Reading feature 0x00
~~~

if option ***--timestamp*** (alternatively ***--ts***) is specified, trace messages are prefixed with the number of seconds since the start of program execution, e.g. 
~~~
[  1.827](ddc_get_vcp_value             ) Starting. Reading feature 0x00, dh=Display_Handle[i2c: fd=3, busno=9], dh->fd=3
~~~

If both options are specified, then both thread id and timestamp are shown. 


#### Mouse corruption during display detection

When examining USB devices to determine if they support the ***USB Monitor Control Class Specification***, it was possible to hang USB connected mice. This change peforms a pre-check to ignore mice and keyboards.  It affects:  
- Normal ***ddcutil*** commands, e.g. ***ddcutil detect***   
- The ***usbenvironment*** command   
- Command ***chkusbmon***, intended for use in UDEV rules.

#### Display Detection messages

When DDC/CI communication fails during display detection and a monitor is reported as an "Invalid Display", 
if the display appears to belong to a laptop, issue a message that laptop displays do not support DDC/CI.  This should reduce the incidence of 
people needlessly trying to diagnose a DDC/CI communication failure.

Command ***ddcutil detect*** now shows a blank instead of "unspecified" to indicate that an EDID is not found. 

#### Miscellaneous

Improve the output for feature x14 (Color Preset) to take into account the MH byte, which is defined in MCCS 3.0 and 2.2. 
( Unfortunately, observed behavior on at least one MCCS 2.2 monitor, an HP Z22i, does not match the spec.)

#### API Changes

See [Shared Library Changes for Release 0.9.8](c_api_98.md) for details.



### 0.9.7

04 September 2019

Fixes the cause of a segfault during display detection if a monitor appearing to support 
the **USB Monitor Control Class Specification**, i.e. one that uses USB to communicate monitor settings, is connected to the system.

Properly handle failure when querying a feature value using USB.

Plug a minor memory leak during display detection when checking if a display is an eDP device, i.e. a laptop monitor.
 Thank you Federico.

Some debug messages still occurring in production code were disabled.

Minor code cleanup to avoid warnings in Coverity static analysis.



### 0.9.6 

25 August 2019

#### Command line option **--nousb**. 

This option disables the USB portion of display detection during **ddcutil** startup.  

There has been a user report that **ddcutil** sometimes freezes the trackpoint in a Lenovo keyboard with trackpoint (specifically Lenovo SK-8855) during display detection.  The option provides a work around.

Note that USB display detection is rarely required, 
as most monitors do not use USB to communicate settings.  However, USB display detection is much, much faster than normal I2C display detection, so disabling it has a negligable effect on **ddcutil** execution time


#### API Changes

See [API Changes in Release 0.9.6](c_api_96.md) for details.


### 0.9.5

24 February 2019

#### Feature x72 (Gamma) 

Added support for Virtual Control Panel feature x72 (Gamma).  This is an unusual complex non-continuous feature in that its value can be changed by the user, and the parenthesized "values" in the capabilities string require special interpretation.  The following commands have been modified:   
- ***ddcutil getvcp 72***:  Interpret the output  
- ***ddcutil capabilities***:  Interpret the parenthesized "values" substring  
- ***ddcutil setvcp***: See below.

#### Command **setvcp** 

Can now take 2 byte values an an argument. 
Values can be specified as hexadecimal (SH byte then SL) or as decimal (0..655335). 

Example: ***ddcutil setvcp 72 x7800***  
Specifies SH=x78 (native gamma=2.2), SL=x00 (absolute adjustment, ideal tolerance) 

Thanks to YamashitaRen for pointing out the need to allow for 2 byte values and helping test the support for feature x72.

#### Configuration and Diagnostics

Contrary to existing documentation, kernel module **i2c_dev** is in fact required when using the proprietary Nvidia driver. 
Reported by Igor Ovsyanka.  Kernel module documentation has been corrected, and command **ddcutil environment** has been updated.

#### Bug Fixes

Fix the cause of an assert failure when displaying the value of feature x8F (Audio Treble) or  x91 (Audio Bass). 
Reported by Igor Ovsyanka.

#### Monitor Quirks

Instead of setting the "unsupported feature" bit in the ***getvcp*** reply packet for an unsupported feature, the Dell AW3418DW causes I2C
failure with Linux errno **EIO**.  **ddcutil** now interprets the **EIO** response as indicating an unsupported feature instead of reporting an error.
This special handling currently applies only to the Dell AW3418DW.

#### Miscellaneous

Update file README.md and make it more usable as the github overview file.

#### API Changes

See [API Changes in Release 0.9.5](c_api_95.md) for details.



### 0.9.4

25 December 2018

Fix the cause of failure to build from source for the [x32 ABI](https://en.wikipedia.org/wiki/x32_ABI).
The file output by the ***dumpvcp*** command no longer contains a ***TIMESTAMP_MILLIS*** field,
the number of seconds since start of the machine epoch, typically January 1, 1970.
This field, and its companion ***TIMESTAMP_TEXT***, are ignored on input, so this change can
only affect an external program that relies on the TIMESTAMP_MILLIS field, say for sorting.
Thank you Jan Palus, who encountered the problem when building **ddcutil** for the PLD Linux distribution.

Fix an execution failure that resulted from attempting to open an already opened display.  This
occured on the ***capabilities*** command when the ***vcp*** field  in the string returned by the 
monitor was damaaged or missing, causing **ddcutil** to attempt to read the VCP version from feature 0xDF.
Thank you Sergiy Domin for reporting this bug.

Features 0x59..0x5E (6 axis separation values).  Report these values as normal continuous (C) values
instead of just uninterpreted bytes.

### 0.9.3 

26 November 2018

#### Display Detection 

Display detection no longer performs initial probes of I2C slave addresses on each possible /dev/i2c device. These initial probes do not return valid information on a Dell P2715Q, and have been seen to cause screen corruption on Dell XPS 13 laptops.
In addition, /dev/i2c devices for Synopsys DesignWare are automatically skipped.

Thank you Robert Munteanu for reporting and helping diagnose the P2715Q issues.
Thank you Federico, developer of [CLightd](https://github.com/FedeDP/Clightd) which uses the **ddcutil** API, jghauser, and especially Dr-Ongo for extensive help in reporting and diagnosing the Dell XPS 13 screen corruption. 

As a result of the changes to display detection, ***ddcutil detect --verbose*** no longer reports the presence of slave addresses x30 (EDID block number) or x37 (DDC), but does report whether a display is an Embedded Display Port (eDP) device, which is the case for recent laptop displays.  It also reports the sysfs device name. 

#### User Supplied Feature Definitions

**ddcutil** internals have been extensively restructured for future support of user supplied feature definitions.  This will allow users to supply feature definitions for manufacturer reserved features, i.e. those in the range xe0..xff.  While not yet ready for public use, there are a couple changes already visible in the user interface

| Command option | Explanation                                 | 
|----------------|---------------------------------------------|
|--udef          |&nbsp; check for user supplied feature definitions |
|--noudef        |&nbsp; do not check for user suplied feature definitions (currently the default) |

<br>
The following feature group has been added:

| Group name     | Explanation                                        | 
|----------------|----------------------------------------------------|
| USER or UDF    |&nbsp; features having a user supplied feature definition |

<br>

#### Tracing

Added trace groups: 

| Name   | Purpose                 |
|--------|-------------------------|
|UDF     |&nbsp; user defined features   |
|DDCIO   |&nbsp; low level DDC functions | 
|VCP     |&nbsp; feature metadata        |

<br>
Because of the volume of output, 
trace group "DDC" has been split into DDC and DDCIO, with the latter containing low level DDC services.


#### API Changes 

The C API has had a few changes to support user supplied feature definitions and reflecting experience gained from work on the Qt C++ GUI interface.
See [API Changes in Release 9.3](c_api_93.md) for details.


#### Miscellaneous

Vebose output of the ***environment*** command now displays execution timestamps to aid in interpreting system logs.


### 0.9.2

01 September 2018

#### Command ***ddcutil detect***

**Eliminate the ***Supports DDC:*** line.**

The output of this line reflects the result of a simple test for slave address x37 at the I2C layer.
During display detection, **ddcutil** also attempts a real DDC exchange at the higher, DDC layer of code, 
and relies on this result to determine whether the monitor supports DDC.  The simple x37 test is vestigial and has no effect on program logic.
It also has been shown to be inadequate.  It has been reported that the Dell P2715Q sometimes
fails the simple x37 test even though it supports DDC.  On the other hand the x37 test succeeds on a Dell U3011
when DDC is turned off in the OSD.  Thank you Robert Muntreanu for the Dell P2715Q report.

The result of the x37 test is still reported as part of the ***--verbose*** output of ***detect***.

**Eliminate duplicate displays for DisplayPort connected monitors.**

Under some circumstances, a DisplayPort connected monitor is assigned two /dev/i2c devices, and
***detect*** would report two (identical) monitors.  However, only one of the /dev/i2c devices supports DDC communication.
This change addresses the problem for the nouveau driver, and only defines a **ddcutil** display for the 
good /dev/i2c device.

#### Command ***ddcutil --help***

Include command ***scs*** (Save Current Settings) in the output of ***ddcutil --help***. 

***ddcutil scs*** executes a Save Current Settings operation, which is supported by some but not all 
DDC capable displays.  This command was implemented some time ago, but was not described in the help output.


#### Command ***ddcutil environment***

When checking for group i2c membership, issue a special message if running as root instead of the
generic message that the current user is not a member of group i2c.

#### USB connected displays

- Fix an invalid feature read of a USB connected display, observed when getting the controller manufacturer
but VCP feature code xC8 is unsupported.
- When reading a USB report, regard errno EINVAL as an expected indication that no 
value is found rather than an unexpected status code.


#### Tracing

- Fix the cause of a segfault when tracing a write-only DDC exchange (i.e. setting a VCP value)
- Disable debug messages regarding device id table initialization  in the ***environment*** command.
- Eliminate double newlines in several error and trace messages

#### Building ddcutil

- libusb function ***libusb_set_debug()*** is deprecated in recent libusb versions, in favor of ***libusb_set_option()***, 
resulting in a compiler warning when flag ***-Wdeprecated-declarations*** is set.  Calls to ***libusb_set_debug()*** in 
file **libusb_util.c** are replaced by iftested code that uses the proper function.
- Fix an unreachable ***assert()*** statement in file execution_stats.c that was detected by compiling 
with option -Werror-switch-unreachable. Thank you Jonathan Scruggs.
- Add "\_\_attribute\_\_ ((deprecated))" statements to functions ***ddca_report_active_displays()*** and ***ddca_get_feature_name_by_display()***, 
in header file **ddcutil_c_api.h**.  The inline documentation for these function already indicated their deprecated status.



### 0.9.1

27 May 2018

#### Bug Fix

Fix the cause of an abort that may occur on command ***environment --verbose*** when examining an extremely large log file.

### 0.9.0

13 May 2018

#### Significant Command Line Changes

Two enhancements will be of interest to general users: 

- The **setvcp** command now allows new values for Continuous type features to be specified as relative values,
e.g. the following commands increase or decrease the value of the brightness feature by 5. 
~~~
$ ddcutil setvcp 10 + 5
$ ddcutil setvcp 10 - 5
~~~
Note that parsing requires that "+" and "-" be surrounded by spaces.

- Option ***--no-table*** is now the default.

- Table type features are by default not included in most feature groups specified on ***getvcp***, e.g. ***getvcp known***.
Features of type Table are rare.  The DDC/CI spec does not provide a clean way for ***getvcp*** to determine that a table 
feature does not exist.  As a result, ***getvcp*** typically has to exceed its retry count before giving up.
(Exclusion of table features does not occur if a feature is explicitly specified by its hex code, 
or for feature groups ***TABLE*** or ***LUT***.)  
The ***--no-table*** (formerly ***--notable***) and ***-show-table*** options explicitly control this behavior.

Note that ***--no-table*** does not apply for subsets TABLE or LUT.   

For subsets SCAN and MANUFACTURER,  the effect of ***--show-table***/***--no-table*** on 
manufacturer-specific feature codes (xe0..xff) is more complicated. **ddcutil** 
has no information about the feature characteristics of manufacturer-specific codes.  Normally, it attempts only a non-table read.  
However, if both ***--verbose*** and ***--show-table*** are in effect, then a table read is attempted as well as a non-table read. 


##### Reporting Unsupported Features

The interpretation of ***--show-unsupported*** has been tweaked.  This option applies to command ***getvcp***.  (The ***probe***
command always reports unsupported features.) 
 Command ***getvcp*** reports unsupported features if any of the following hold:  
- A specific feature was specified by its code, as opposed to a feature set.  
- Verbose output is in effect  
- Option ***--show-unsupported*** was specified.

Option ***--show-unsupported*** is no longer automatically set for feature set ***ALL***, which is now simply a synonym for ***KNOWN***.


##### Options ***--rw***, ***--ro***, ***--wo***

Options ***--rw*** and ***--ro*** apply to both the **getvcp** and **vcpinfo** commands.  Option ***--wo*** applies only to **vcpinfo**. 

If specified, features shown are restricted to those that are read-write, read-only, or write-only. 

For example: 
~~~
$ ddcutil getvcp all --ro
~~~

##### Option ***--mccs***

Filters the information returned by **vcpinfo** to that for the specified MCCS version.  For example, 
~~~
$ ddcutil vcpinfo --mccs 2.1
~~~

- Feature selection for **ddcutil vcpinfo table** is not properly version sensitive.  
  -- take version from monitor selection if specified    (VERIFY)  
  -- option --mccs-version to force specific version

##### Feature Set Identifiers

Additional feature sets have been defined for the **getvcp** and **vcpinfo** commands.
They surface internal **ddcutil** feature descriptions, and are intended to facilitate exploring the MCCS specification and its implementation on 
particular monitors.  They will likely be of interest to only a few user.


|Feature Set | Description                 |
|---------|-------------------------------------|
| SCONT   | Simple Continuous features |
| CCONT   | Complex Continuous features |
| CONT    | All Continuous features |
| SNC     | NC features having a defined set of features for byte SL |
| CNC     | NC features using multiple bytes |
| NC_WO   | NC features that are Write|Only |
| NC_CONT | Features of type NC that reserve specified values as flags, alloting the remaining values to a continuous range |
| NC      | All Non-Continuous features |

MFG is now a valid synonym for feature set MANUFACTURER.  
Feature sets KNOWN and ALL are now synonyms.    

#### Miscellaneous

- The VCP file created by **dumpvcp** now includes a **PRODUCT_CODE** field including the manufacturer product code from the EDID.
For some manufacturers, the model name in the EDID does not in fact distinguish among models. Notably, Samsung commonly uses just 
"Syncmaster" for the model name.  Currently, **loadvcp** recognizes this field name but does not make use of the value.

#### Building ddcutil 

- Eliminate compiler flag ***-Wpedantic*** in Makefiles.  This flag had been added to diagnose some obscure compiler warnings regarding 
system header files. It was flagging perfectly valid code as problematic, and caused build failures on Gentoo. 
- Code changes to avoid ***-wformat-overflow***, ***-wformat-truncation***, and ***-wstringop-trunction*** warnings introduced with GCC 8, 
generally by making buffers larger.
(All such warnings had flagged situations that never would result in buffer overflow.)


#### API Changes 

The C API has been extensively revised reflecting experience gained from work on a Qt C++ GUI interface.
See [API Changes in Release 9.0](c_api_90.md) for details.

Work on Python API has been suspended.  Proof of concept implementations exist in the source tree for implementations using GObject API. 
SWIG, CFFI, and and Cython.  However, these are incomplete and not intended for public use.


### 0.8.6

20 January 2018

The externally visible changes in this release include minor enhancements and bug fixes.  There are also 
changes in the C API.  Internally, there have been extensive changes in support of the C and (future) Python APIs.

#### User Interface Changes

Because of the design of the DDC/CI protocol, there is no certain way to distinguish a response indicating that 
a feature of type Table (T) is unsupported from a DDC/CI protocol error.  As a result, ***ddcutil getvcp*** performs the maximum 
number of retries on a Table type feature before giving up.  However, Table type features are rarely implemented.  The new ***--notable*** option allows Table type features to be ignored, speeding up execution of ***getvcp*** commmands for multiple features.

- Option ***--notable*** applies if the argument to ***getvcp*** is a feature set, e.g. ***COLOR*** or ***KNOWN***
instead of a single feature id.  In that case Table type features are ignored.

#### C API changes

- #### Replace structs that use #iftests
Structs **DDCA_Single_Vcp_Value** and **DDCA_Non_Table_Value_Reponse** used preprocessor #iftests for
endian-ness to properly overlay the 2 byte continuous values (e.g. max value)
on a pairs of 1 byte NC values (e.g. ml, mh).  The use of #iftests proved incompatible with 
Python C function interface systems (e.g. Cython, CFFI).
  
- Struct **DDCA_Single_Vcp_Value** has been replaced by struct **DDCA_Any_Vcp_Value**. 
- Function **ddca_get_single_vcp_value()** has been replaced by fuction **ddca_get_any_vcp_value()**, 
which uses **DDCA_Any_Vcp_Value**.
- Struct **DDCA_Non_Table_Value_Response** has been replaced by struct **DDCA_Non_Table_Value**.
- Function **ddca_get_non_table_value()** now returns a **DDCA_Non_Table_Value**, not a **DDCA_Non_Table_Value_Response**. 

**DDCA_Any_Vcp_Value** and **DDCA_Non_Table_Value** do not define 2  byte current and max values for Continuous (C) 
features.  The 2 byte current and max values must be calculated from the appropriate pairs of single byte values, e.g.
~~~
cur_val = sh << 8 | sl
~~~

- #### Eliminate use of longjmp
The use of longjmp() to handle exceptional error condiitions (typically program logic errors) has been eliminated.

- API functions removed: **ddca_register_jmp_buf()**, **ddca_get_global_failure_information()**
- Structs removed:  **DDCA_Global_Failure_Information**


#### Miscellaneous

- Add trace class ***ENV*** for tracing environment related functions
- Internally, many functions in key portions of the code base now return exception-like structs 
  instead of status codes.  Option ***--excp*** causes ddcutil to report these exception-like structs
  when they are converted to status codes (and the internal detail is discarded).

#### Bug Fixes

- Fix cause of a segfault in the ***environment*** command on Arch Linux due to the lack
  of the "arch" command in that distribution.  Thank you Despruk.
- Fix a segfault when probing DRM using the ***environment*** command on aarch64.
- Fix a typo in the recommendations section at the end of the ***environment*** command. Thank you Mirek Svoboda


### 0.8.5

16 November 2017

This release contains a large number of minor enhancements and bug fixes.  Most significant
are those related to alternative environments, such as Raspberry Pi systems.

Those running **ddcutil** on a 32 bit system, either i386 or Raspberry Pi, should
upgrade to address a segfault in the ***environment*** command.
Otherwise, there is no need to upgrade to this release unless you are experiencing problems. 

#### Changes to facilitate scripting

- Do not output a warning message regarding undetected VCP version if message level is ***--terse***.
- "Display not found" message now directed to stderr instead of stdout.

#### Packaging

- Changes to meet Debian distribution standards.  Clean up debian/rules and other files in the debian directory.
- Add a changelog file, normally installed in file /usr/share/doc/ddcutil/Changelog.
- Rename NEWS file to NEWS.md
- Add ***configure*** option ***--enable-x11***/***--disable-x11***.  The default is ***--enable-x11***. 
If ***--disable-x11*** is specified, **ddcutil** is built without X11 related code, which is 
used only for diagnostics within the ***environment*** command.
- Directory ***package*** has been removed from the **ddcutil** git tree and is no longer included
in the tarball.  This more cleanly separates "downstream" from "upstream" activities.
Packagers for Linux distributions should contact if this creates problems.

#### Command **environment** enhancements

The ***environment*** command is extensively revised.   The increasingly large collection of 
tests has been consolidated.  In particular:

- Tests are now sensitive to machine architecture.  Performs alternative tests on arm systems,
inlcuding the Raspberry Pi. 
- Added a consolidated collection of suggestions at the end that suggest ways for the 
user to address configuration issues.
- Fixed a bug that caused a segfault in 32 bit versions of **ddcutil**. 
- Use of shell commands has largely been replaced with API calls, addressing some obscure 
shell errors.

#### glib-2.0

The required glib-2.0 version has been increased from 2.16 to 2.32 reflecting the use of glib 
thread functions.  Consequently, **ddcutil** no longer builds on some older Linux distributions.

#### Bug fixes

- Fix a logic error in the VCP version comparison function vcp_version_le().  This bug affected only audio 
related VCP feature codes x8f, x91, and x93.  It also affected experimental command ***watch*** which listens for monitor
changes not made by **ddcutil**, i.e. by pressing buttons on the monitor.
- Properly handle the failure case where a display is detected on an I2C bus (EDID is read) but DDC communication fails, 
and the user specifies the display on the command line using the I2C bus number (option ***--bus***).
- Fix the cause of a segfault when the ***loadvcp*** command reads a user modified VCP file.  The 
command now fails gracefully if none of MFG_ID, MODEL, and SN are present.
- Fix the cause of a segfault when displaying I2C functionality flags in the --environment command.  This segfault
was seen only in 32 bit versions of **ddcutil**.
- The --stats option showed incorrect time statistics on 32 bit versions of **ddcutil**.
- Add "-lddcutil" to output of "pkgconfig ddcutil --libs".  Thank you Federico.

#### Miscellaneous

- Improve reporting of the individual errors that result in command failure due to maximum I2C retries exceeded.
- ***interrogate***, ***environment*** and ***usbenvironment*** now redirect all stderr output to stdout for easier capture
- Added command line options ***-trcfile*** and ***-trcfunc*** to enable tracing by file name and function name.
- Added configuration option ***--enable-x11*** controlling whether X11 related diagnostics are included in the ***environment***
and ***interrogate*** commands, allowing **ddcutil** to build in embedded environments that lack X11.  The default is ***yes***.
- Verbose EDID decription, e.g. on command ***detect --verbose***, now reports EDID byte 24 (x14), the supported features bitmap.
This is purely informational.
- Ongoing work to make functions thread safe. 
- Code cleanup.


### 0.8.4

22 July 2017

This release primarily contains packaging changes to meet Fedora distribution standards.
It also includes minor improvements and bug fixes. 
There is no need to install it unless you are experiencing problems.

#### Command **ddcutil interrogate**

- Do not include **usbenvironment** diagnostics as part of the **interrogate** command. USB connected monitors 
  are rare and USB related diagnostics clutter the report.

#### libddcutil-devel

- Do not package static libraries (.a and .la files) in libddcutil-devel. 

#### Bug fixes

- Prevent a segfault in monitor detection that occurrs when slave address x50 is detected on an I2C bus, but reading the EDID at that address fails.

- Address an assert failure in the **ddcutil usbenvironment** command due to an unexpected library return code.

- In the device identification cross reference report of **ddcutil environment**, properly handle I2C bus number -1, used internally to 
  indicate unknown.


### 0.8.2

17 May 2017

This release introduces minor improvements to the diagnostic output of the **environment**, 
**detect**, and **interrogate** commands.  There is no need to install it unless you are 
experiencing problems.

#### Command **ddcutil detect**

- If a display appears to support DDC (I2C slave address x37 is active) but communication fails, 
  perform a heuristic test as to whether it is a laptop display, issue message appropriately.
- Changes to reduce the number of feature reads performed when detecting monitors, slightly 
  improving performance.

#### Option **--stats errors**

- Maintain and report a secondary count of the individual errors that are rolled up into "retry count exceeded" errors.

#### **ddcutil environment --verbose** changes

  - Eliminate checks for package **i2c-tools**
  - Report additional information about system hardware
  - Add a cross reference report for the multiple ways that different subsystems identify 
    a monitor and its I2C bus
  - Consolidate some tests to reduce redundancy
  - Check for DisplayPort Multi-Stream Technology (DPMST) devices
  - Eliminate the cause of a bogus error message when scanning devices

#### Packaging

- Changes to the requirements of package **i2c-tools**.  Generally speaking, it is no longer needed for 
  building **ddcutil**, and is optionally used as part of the diagnostic output of the **environment** command.
    - Changed from **Requires:** to **Suggests:** for dpkg
    - **Requires:** and **BuildRequires:** eliminated for rpm packaging in general
    - **BuildRequires:** kept for SuSE rpm packaging.  **ddcutil** needs file linux/i2c-dev.h, which is 
      contained in package **i2c-tools** in SuSE releases through 13.2
- Changes to packaging to conform to Debian standards
- Changes to packaging to conform to Fedora standards


### 0.8.1

06 May 2017

This is a bugfix release.  There is no need to install it unless you are experiencing the 
segfault described below.

- Fixes a segfault that can occur after open() fails for a /dev/usb/hiddev device.  

### 0.8.0

01 May 2017

Release 0.8.0 contains new features intended to address issues with particular monitors and user environments,
to changes to improve performance, and code restructuring for future enhancements


#### General Changes

- Monitor detection and cataloging has been extensively reworked. 
To support the library version of **ddcutil** used by the C (and future Python)
APIs, the list of displays is determined at program/library initialization and 
saved.  


- The processing of the DDC Null Response has changed.  Normally a Null Response
indicates a processing error in the monitor.  Some monitors also use the Null Response
to indicate that a VCP feature is unsupported.  **ddcutil** now performs an 
extended sleep when receiving a Null Message response to allow the monitor 
to recover.  It also performs an initial check of whether a monitor uses DDC Null Response to 
indicate unsupported features. See [DDC Null Response](ddc_null_response.md).

- Linux error numbers no longer appear "modulated" in messages.  That is, an EACCES
error is reported as -13, not -1013.  Note that the C API has always returned 
Linux error numbers in unmodulated form.


#### General Options

Option **--async** parallelizes the monitor checks executed during display detection. 
If there are multiple monitors, initial monitor checks are performed
in multiple threads, improving performance.  This may become the default in future releases.

Option **--nodetect** is valid if the display is specified by its I2C bus number, using option **--bus**.
In that case the initial detection of all displays is skipped, improving performance.
This may become the default in future releases.

Option **--hiddev <number>** specifies a USB monitor connection by its hiddev device number.  For example:
~~~
--hiddev 3
~~~
refers to device /dev/usb/hiddev3.  There is no single letter 
specifier for this option.

Option **--brief** is now a synonym for **--terse**

Added **--stats** class **elapsed**.  **--stats time** is a synonym for **--stats elapsed**.  Shows program 
elapsed time.  

#### Changes to Specific Commands:

**environment** and **interrogate** commands:

- Report ddcutil version


**probe**, **interrogate**, and **getvcp scan** commands:

- Do not include table features unless output level is VERBOSE. 
This speeds up execution as table reads for unsupported features 
execute the maximum number of retries.  It also clarifies performance statistics.
No monitors implementing table type features have yet been observed.

**interrogate** command:  

- Report performance statistics for individual parts of **interrogate**.

**getvcp** command:  

- If **--terse** output is specified, then the VCP data is output in a form that
is easily machine readable.  See [getvcp command](command_getvcp.md)

**setvcp** and **loadvcp** commands:

By default, **setvcp** and **loadvcp** verify that a value has been successfully
set by reading the value from the monitor after writing it.  Option **--noverify** 
disables this check.

#### Bug Fixes
- Fixed a segfault that occurred when a monitor returned a 0 length capabilities string.
- Variables holding nanoseconds have been changed from type **long** to **uint64_t**
to address overflow problems on 32 bit systems such as Raspberry Pi.



#### C API 

The C API has undergone extensive changes.

#### Name Changes

Names have been changed for consistency and clarity.
Names of certain functions used in message generation have been shortened 
to simplify printf() type functions. 

|Old typedef name | New typedef name
|-----------------|-------------------
| VCP_Feature_Code  | DDCA_Vcp_Feature_Code
| Version_Feature_Info | DDCA_Version_Feature_Info
| Vcp_Value_Type       | DDCA_Vcp_Value_Type
| Single_Vcp_Value     | DDCA_Single_Vcp_Value

|Old function name | New function name
|-----------------|-------------------
| ddca_status_code_name()  | ddca_rc_name()
| ddca_status_code_desc()  | ddca_rc_desc()
| ddca_repr_display_identifier() |  ddca_did_repr()
| ddca_create_display_ref()      |  ddca_get_display_ref()
| ddca_repr_display_ref() | ddca_dref_repr()
| ddca_repr_display_handle() |ddca_dh_repr()
| ddca_get_displays() |  ddca_get_display_info_list()
| ddca_repr_mccs_version() | ddca_mccs_version_id_name()
| ddca_mccs_version_id_string() | ddca_mccs_version_id_desc()

Comments:  
- Function **ddca_create_display_ref()** was renamed to **ddca_get_display_ref()** to reflect 
the fact that it returns a **DDCA_Display_Reference** from the internal data structures 
representing detected displays.



|Struct              |Old field name |New field name
|-------------------|----------------|-----------------
|DDCA_Ddcutil_Version_Spec |build     | micro


|Enum                 |Old value name   | New value name
|---------------------|------------------|----------------
|DDCA_Output_Level    | OL_TERSE          | DDCA_OL_TERSE
|DDCA_Output_Level    | OL_NORMAL        | DDCA_OL_NORMAL
|DDCA_Output_Level    | OL_VERBOSE         | DDCA_OL_VERBOSE
|DDCA_IO_Mode         | DDC_IO_DEVI2C      | DDCA_IO_DEVI2C
|DDCA_IO_Mode         | DDC_IO_ADL     | DDCA_IO_ADL
|DDCA_IO_Mode         | USB_IO            | DDCA_IO_USB
|DDCA_Vcp_Value_Type  | NON_TABLE_VCP_VALUE |  DDCA_NON_TABLE_VCP_VALUE
|DDCA_Vcp_Value_Type  | TABLE_VCP_VALUE | DDCA_TABLE_VCP_VALUE


#### Structs and Functions Removed

| Structs removed | Comment
|-----------------|--------------
| DDCA_IO_Location | Use DCCA_IO_Path


|Functions removed | Comment
--------------------|------------
| ddca_built_with_adl() | Use ddca_get_build_options()
| ddca_built_with_usb() | Use ddca_get_built_options()

#### Enums, Structs, and Functions Added or Changed

| New enum           | Purpose
|------------------------|-------------------------------
| DDCA_Stats_Type    | Used as both values and bitflags for statistics types

| New Structs        | Purpose
|-------------------| -----------------
| DDCA_Adlno        | Represents a ADL adapter number/display number pair
| DDCA_IO_PATH      | Physical path for MCCS communication

Comments:  

- Struct **DDCA_Display_Location** is replaced by **DDCA_IO_Path**, 
which contains only the io mode and device identifiers 
for that mode.  USB bus and device numbers were removed
because they are attributes by which the physical path 
(hiddev device) can be found, not the path itself.
- Struct **DDCA_Display_Info** has been adjusted to use **DDCA_IO_Path**.

| Function added                              | Purpose
|---------------------------------------------|----------------
| ddca_ddcutil_version()                      | gets version as a struct of numbers
| ddca_reset_stats()                          | reset performance statistics
| ddca_show_stats()                           | report performance statistics
| ddca_enable_verify()                        | controls whether VCP values are read after being set
| ddca_is_verify_enabled()                    | queries whether VCP values are verified
| ddca_free_display_info_list()               | free function for struct
| ddca_create_usb_hiddev_display_identifier() | create identifier for hiddev number
| ddca_get_simple_nc_feature_value_name()     | lookup name of simple NC feature value


#### Other Changes

Functions now return Linux error number -EINVAL rather than DDC specific 
error number DDCL_ARG in case of invalid argument 

The display selection lifecycle has been changed.  The **DDCA_Display_Identifier** is not 
the only way to obtain a **DDCA_Display_Reference**. 
It can be found in one of 2 ways:  

- Function **ddca_get_display_info_list()** returns  a collection **DDCA_Display_Info**, containing 
display identification information (e.g. ddcutil assigned display number, I2c Bus number, manufacturer id, etc.) and a display reference.
The client program can search through the returned collection to find the
desired monitor.  
- A **DDCA_Display_Identifier** can be created and passed to ddca_get_display_ref(), which returns 
a **DDCA_Display_Reference**.


See sample program **demo_display_selection.c** for examples. 



#### Sample code files:   

The following example files for using the C API have been added:

| File name                |  Purpose 
|--------------------------|-------------
| demo_global_settings.c   | Report and modify global settings  
| demo_display_selection.c | Display selection  
| demo_vcpinfo.c           | Query VCP feature code descriptons  
| demo_get_set_vcp.c       | Get and set VCP values

File **clmain.c** is now a miscellaneous catchall, mainly for testing.


#### Build system changes

- src/Makefile.am: change dist-hook to use -prune option per Knut Omang; 
avoids eror in case there is nothing to erase (i.e. build hasn't happened)
- Added cmake file FindDDCUtil.cmake from Dorian Vogel


### 0.7.3

05 March 2017

Maintenance release.

- Add additional diagnostic tests to command **ddcutil environment --verbose***. 
- New configure option ***--enable-drm***.   
    - If enabled (the default), command **ddcutil environment --verbose** includes additional tests that use
DRM services to better analyze disrepancies observed in the field between the EDID read directly from the monitor and that reported by X11.
    - Option ***--enable-drm*** requires that a recent version (>= 2.4.67) of the development package for libdrm2 (libdrm-dev or libdrm-devel depending on Linux distribution) be
installed when building **ddcutil**.  If a sufficently recent version is not detected, **configure** issues a warning and reverts to ***--disable-drm***.
    - If minimizing executable size is important, set ***--disable-drm***, as DRM services are used only for diagnostics.
- Additional information on **ddcutil detect --verbose**.
    - Report and interpret USB vendor id/product id when **detect** cannot open a hiddev device.  This is usually benign, and identifying the device should make this evident.
    - Suggest that the user check that DDC/CI is enabled in the monitor's on screen display if I2C bus x37 (DDC) is responsive, but DDC communication fails.



### 0.7.2

01 February 2017

Maintenance release.

- Fixes a critical bug in release 0.7.1 where insufficient privileges on a /dev/i2c-n device causes program termination.
- **ddcutil interrogate** always executes with ***--set-slave-address*** in effect
- Minor improvements to **ddcutil environment** and **ddcutil interrogate** output
- Minor improvements to **ddcutil detect --verbose**
- Add explanations for additional ***errno*** values


### 0.7.1

27 January 2017

Mantenance release.

- Add option ***--force-slave-address*** which causes ddcutil to take control of slave addresses on the 
I2C bus even if they are in use.  Internally, ***ioctl(I2C_FORCE_SLAVE)*** is called instead of ***ioctl(I2C_SLAVE)***.
This is intended as a possible workaround for situations where monitors on an I2C bus are not detected. See [Option --force-slave-address](other_options.md#force_slave)
- Option ***--force*** disables various checking.  It's use is likely to result in command failure, but may 
provide useful information as to the source of problems.  See [Option --force](debug_options.md#slave)
- Fix the cause of a segfault when displaying  statistics for option ***--stats errors*** or simply ***--stats***, or on 
the **interrogate** command.
- **make install** will not install C header files unless configuration option ***--enable-lib*** is set.
- Fix the cause of a Makefile error when trying to strip dependency information from **libddctuil.la** during **make install**.
- Extensive rework of Autotools files for future support of Python 3 as well as Python 2.  This work is incomplete.
- Emit diagnostic messages to aid in debugging certain situations seen only in the field.

### 0.7.0

03 January 2017


- [C API](api_main.md)
    - Should be fairly stable, though not all features of **ddcutil** are exposed.
    - Reflecting the presence of APIs, the default for the ***--enable-lib*** configuration option is now "yes".
      The following shared object library is created: libddcutil.so
    - Additional prebuilt packages: libddcutil0, libddcutil-dev (Debian, Ubuntu) or ddcutil-devel (Fedora, SUSE)
- [Python API](api_main.md)
    - The Python API, implemented using SWIG, is a rough proof of concept and subject to major change.
    - SWIG support is only available by building from source, not from pre-built packages.  Requires ***--enable-swig*** configuration option.  
- A new command line option ***--mfg***, which allows for monitor selection using the 3 character manufacturer id 
  found in the EDID.  ***--model*** and ***-sn*** no longer need to specified together.  If any of ***--mfg***, ***-model***, and ***-sn***
  are specified, the first monitor to match all of the manufacturer/model/serial-numbers that are specified is chosen.
- The monitor feature and capabilities portion of ***interrogate*** has been enhanced.  This functionality is also exposed using the 
  new ***probe*** command which operates on a single monitor.
- Parsing of the capabilities string is enhanced to accomodate monitors that do not separate feature codes by blanks.



### 0.6.1

21 November 2016

Maintenance release.  

- Improve recovery and diagnotistic messages for certain exceptional conditions. 
- Command **ddcutil interrogate** now reports the differences between VCP codes 
declared in the capabilities string and those observed by scanning. 
- Extensive internal changes in preparation for future C and Python APIs.

### 0.6.0

01 October 2016

Rename project from ddctool to ddcutil.  

- Primary executable is now named ddcutil  
- Shared library is now named libddcutil.so
- Tarball is now named ddcutil.n.n.n.tar.gz

### 0.5.3

24 September 2016

Fix overzealous code cleanup in release 0.5.1.  

- Undefined reference to function is_module_loaded_using_sysfs() when building without ADL support.  
- Error in command parsing. Command arguments were ignored.

### 0.5.1

23 September 2016

Minor improvements to diagnostics of the **environment** and **interrogate** commands:

- Check if i2c_dev is built into the kernel as an alternative to it being a loadable kernel module  
- Recognize amdgpu video driver


### 0.5
09 September 2016

This is the first formal release of **ddcutil**

- Rework USB monitor support based on testing with Eizo Coloredge and NEC PA series  
-- Probing of USB connected monitors is greatly extended and refactored from **ddcutil environment** into a separate command **ddcutil usbenv**.
Libraries hidraw and libusb are used as well as hiddev.  HID Report Descriptors are parsed.  This probing is included in **ddcutil interrogate**.   
  -- **configure** option **--enable-usb/--disable-usb** controls whether ddcutil is built
  with USB monitor support.  The default is **--enable-usb**.  When building with USB support,
  packages for udev, hidraw, hiddev, and libusb are required.
- Simplify README.md.  Refer to www.ddcutil.com for most documentation  
- The assumption that I2C buses are numbered consecutively is removed.  Required for Raspberry Pi. 
 - Allow a display to be specified on the command **ddcutil loadvcp**.
 This addresses the situation where a monitor presents both an I2C and a USB interface.
 Normally, the display is determined using the data stored in the VCP file, and the I2C display interface is
 chosen over the USB interface.
 If a display is specified, then the model and sn from its EDID must match
 those in the VCP data being loaded.  
- Add **ddcutil** command option **--timestamp/--ts**.  If specified, trace messages are prefaced with an elapsed timestamp.
- Because of changing package requirements, **ddcutil** no longer builds in the openSUSE Build Service (OBS) for older OS versions (openSUSE 13.1, Ubuntu 12.04, 14.04). 
- **ddcutil** currently does not build for any openSUSE version in the openSUSE Build Service due to violation of policy guidelines.  This problem will
be addressed in a subsequent release. openSUSE users can build ddcutil from its tarball.
