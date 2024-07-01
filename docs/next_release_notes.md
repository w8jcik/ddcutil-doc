## Notes for the Next **ddcutil** Release (Draft)


### Initialization Performance Tuning

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

### Dynamic Sleep

  - Always set sleep multiplier to at least 1.0 for commands **setvcp** and **scs**. Addresses reports
    that aggressive optimization caused setvcp to fail.

### Cross-Instance Locking

The cross-instance locking feature uses system call flock() to
ensure that operations on /dev/i2c devices by multiple instances of **ddcutil** or **libddcutil** do not interfere.
All instances must use option ***--enable-cross-instance-locks***. 

#### Switching input source on recent LG monitors

Option ***--i2c-source-addr*** causes **setvcp** to use the specified address instead 
of the value x51 given in DDC/CI specification.  User experimentation has found that 
using an alternate source address accesses additional features on some LG monitors. 
See [Switching Input Source on LG Monitors](https://github.com/rockowitz/ddcutil/wiki/Switching-input-source-on-LG-monitors).

### Miscellaneous Changes

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
  - do not report "DDC communication failed"
  - Report "Is laptop display" instead of "Is eDP device" or "Is LVDS device"
    
### Building

- **configure** option ***--enable-asan*** added. Turns on ASAN related compiler options and 
  links libasan into binaries.
- **configure** option ***--enable-x11*** is deprecated and has no effect.  The X11 API is no longer used.
  Distribution packagers should eliminate requirements for X11 related libraries.  The option will
  be completely removed in a future release.

