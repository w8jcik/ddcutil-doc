## Frequently Asked Questions

### **ddcutil** does not identify a DDC/CI capable monitor.

Check the following:

- Is DDC/CI enabled in the monitor's On Screen Display?
- Is driver **i2c-dev** loaded?
- Does the current user have permission to access the I2C bus associated with the monitor?

Command **ddcutil environment** will analyze your system environment and make suggestions.
See [enviroment Command](seconday_commands.md/#environment)

### **ddcutil** doesn't work on my laptop.

Laptop displays do not support DDC/CI.

<a name="faq_edp"></a>
### But recent laptops use Embedded Display Port (eDP) panels which have an I2C interface.  Why doesn't **ddcutil** work?

The I2C connection on eDP panels implements slave address x50, which allows programs to read the EDID in the expected way.  However, it does not implement slave address x37, i.e. it does not support DDC/CI. 

If eDP panels did implement DDC/CI, **ddcutil**  would support it.  For each I2C bus that could possibly be associated with a monitor, as part of display detection ddcutil attempts to read the EDID at slave address x50.  If successful, it then attempts to communicate on slave address x37.  If that fails, and display detection is occurring in the context of ***ddcutil detect --verbose***, ddcutil then looks for a possible reason, and checks to see if the display is a laptop.  If so, it then reports that laptops to not support DDC/CI.


<a name="faq_slow"></a>
### **ddcutil** is slow.

Typically, **ddcutil** spends 90% of its elapsed time in waits mandated by the DDC/CI protocol.

Two options can have a big impact on performance:

- Option ***--bus***.  If the /dev/i2c bus number of the monitor is given, **ddcutil** skips the
initial phase of searching the /dev/i2c devices for monitors.  Note that if this option is 
specified, no other monitor selection option such as ***--display*** should be specified.

- Option ***--sleep-multiplier***.  This option adjusts the length of time **ddcutil** spends in 
DDC/CI mandated waits.  For example, if the DDC/CI protocol specifies a 40 ms wait between the 
time a command is sent to the monitor and the time a reply is read, and ***--sleep-multiplier .2*** is 
given, **ddcutil** will only wait (.2 x 40 ms) = 8 ms.  Some monitors have been found to communicate
successfully with ***--sleep-multiplier*** values as low as .1. On the other hand, some monitors 
with poor DDC/CI implementations perform better if the sleep time is increased by using a value greater
than 1.


<a name="docking"></a>
### **ddcutil** can't communicate with a monitor when it's plugged into a docking station. 

**ddcutil** does work when the monitor is plugged directly into the laptop.
The problem affects DisplayPort, DVI, and HDMI connectors on the dock.

This is a problem with newer docking stations that implement DisplayPort Multi Stream Transport.  Until kernel release 5.11, the drm driver layer
did not implement I2C writes. 

For the gory details, see this [thread](https://lists.freedesktop.org/archives/intel-gfx/2017-May/127350.html) on the Intel-gfx developers list. 

<!--


A [fix for this problem](https://cgit.freedesktop.org/drm-misc/commit/drivers/gpu/drm/drm_dp_mst_topology.c?id=adb48b26985686f93f20ca71c16c067d790e7af3) has been developed. It is merged into kernel release 5.11. 


<a name="faq_displayport"></a>
### DisplayPort communcation fails, but DVI/HDMI work

DP<->DP vs DP++ Dual mode DP<->DVI/HDMI

DP<->DVI/HDMI using Active Converted

DP 1.1 vs 1.2

[MegaChips STDP2600 Advanced HDMI to DisplayPort converter Datasheet](https://media.digikey.com/pdf/Data%20Sheets/MegaChips%20PDFs/STDP2600_DS.pdf)
04-March_2916

- for use in active converter dongle
- I2C to AUX bridge for EDID/MCCS patth through
- Disp;ayPort DualMode
  - AUX CH 1 MBPS
  - HDMI/DVI operation with level transmitter
- HDMI input -> DP++ output
  - input: HDMI signal, CEC, DDC
  - output DP Main Lanes, AUX CH
    - SST format only, no MST format support






-->


### I'm using Nvidia's proprietary driver.  **ddcutil** doesn't seem to be be working.

Symptoms include "DDC communication failed" on **ddcutil detect**, and lots of "Maximum retries exceeded" errors
on command **ddcutil getvcp known**.  

You may need the Nvidia secret handshake.  See [Special Nvidia Driver Settings](nvidia.md).

### The ***capabilities*** command reports the MCCS (aka VCP) version, but elsewhere **ddcutil** says the version is unknown.

DDC/CI has two ways to get the the MCCS version:

- As part of the capabilities string returned in response to a DDC Capabilities Request.  
- As the response to a VCP Feature Request for feature xDF (VCP Version). 

Sometimes they disagree.

Command ***ddcutil capabilities*** reports the response to the DDC Capabilities Request.  However, **ddcutil** regards this 
output as purely informational.  (See [capabilities command](command_capabilities.md)) 


**ddcutil** therefore relies on VCP feature code xDF to determine the VCP version.  (For USB connected monitors, it queries HID usage x00800004.)  It is possible that feature code xDF is unsupported, even though the capabilities response specifies a version.

### The output of the ***capabilities*** command is incorrect. 

For example, a Dell P2210 monitor has VGA, DVI, and DisplayPort inputs, but capabilities only reports: 

~~~
 Feature: 60 (Input Source)
    Values (unparsed): 01 03
    Values (  parsed):
       01: VGA-1
       03: DVI-1 
~~~

The capabilities string reported by a monitor is often incorrect. While the ***ddcutil capabilities*** command parses and
reports the string, this is solely informational. 

The only way to know for sure if a monitor supports a VCP Feature Code is by testing using the ***getvcp*** and ***setvcp*** commands. 

The following command will attempt to read all VCP codes that **ddcutil** understands, other than those that are write-only: 

~~~
ddcutil getvcp known
~~~

To attempt to read all possible VCP codes, whether understood by **ddcutil** or not, except for those that are known to be write-only: 
~~~
ddcutil getvcp scan
~~~

To see all the values defined for a non-continuous (NC) feature code, i.e. one with discrete values, use the ***vcpinfo*** command with the 
***--verbose*** option.  For example, to see all the values defined in the **Monitor Control Command Specification** for VCP feature x60 (Input Source): 

~~~
# ddcutil vcpinfo 60 --verbose

VCP code 60: Input Source
   Selects active video source
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: VGA-1
      0x02: VGA-2
      0x03: DVI-1
      0x04: DVI-2
      0x05: Composite video 1
      0x06: Composite video 2
      0x07: S-Video-1
      0x08: S-Video-2
      0x09: Tuner-1
      0x0a: Tuner-2
      0x0b: Tuner-3
      0x0c: Component video (YPrPb/YCrCb) 1
      0x0d: Component video (YPrPb/YCrCb) 2
      0x0e: Component video (YPrPb/YCrCb) 3
      0x0f: DisplayPort-1
      0x10: DisplayPort-2
      0x11: HDMI-1
      0x12: HDMI-2
~~~

In practice, any given monitor will implement only a few of the NC values, and some monitors will implement undocumented values. 
The only way to know for sure is by testing. 

### ***ddcutil detect --verbose*** reports that both I2C address x50 (EDID) and x37 (DDC) are responsive, but DDC communication fails.

There can be any number of reasons for this situation.

- Check that DDC/CI communication is enabled in the monitor's on-screen display?
- It has been observed in the case of a Tegra video card using the nouveau driver.
- See also the discussions of [duplicate entries for a DisplayPort monitor](#duplicate_displayport), and of [failures with newer docking stations](#docking).

<a name="duplicate_displayport"></a>
### The same DisplayPort connected monitor appears twice in the output of ***ddcutil detect***.

Sometimes the same DisplayPort connected monitor is detected at 2 different I2C bus numbers.  ***ddcutil detect*** reads the
EDID at on both buses, but typically DDC communication succeeds on only 1 bus.  **ddcutil** attempts to filter out the invalid bus.
The invalid bus appears to be related to the DisplayPort multistream facility.  You can ignore the invalid entry.


### The ***detect*** command reports different EDIDs depending on how the monitor is connected to the video card.

Strange as it may seem at first, a monitor can have more than one EDID.
The EDID for a VGA connection will have a different video input definition (byte x14) than that for a  digital connection.
The VGA version of the EDID will have timing information.

### The ***detect*** command reports that I2C bus address x30 (EDID block number) is inactive.

EDID version 1.4 allows for addtional 128 byte EDID blocks.  I2C bus address x30 specifies 
which block to read.  These blocks generally contain extended timing information.
**ddcutil** is interested only in the contents of the first EDID block.
The check of bus address x30 is purely informational.

<!--
### **ddcutil** does not work on a Raspberry Pi 4

The HDMI related chip has been changed on the Raspberry Pi 4, and drivers have not yet been revised to enable userspace access to /dev/i2c-2.
For details, see page [Raspberry Pi](/raspberry). 
-->

### **setvcp** does not change the feature value

Several Iilyama monitor models require that command ***scs*** (Save Current Settings) be issued immediately after a feature
value is set.  ***setvcp*** must be invoked with the ***--noverify*** option, followed immediately by ***scs***.

### The monitor supports Dynamic Contrast Range (DCR). **setvcp** for feature x10 (brightness) does not change the value

At least some monitors with Dynamic Contrast Range (DCR) disable feature x10 (brightness) and possibly other features 
when DCR is enabled. This has been reported with both Lenovo and Benq monitors.

### The new value for feature x10 is different from the value specified on **setvcp**

It is common for the new value of a Continuous feature to be off by 1 from the specified value.
This appears to be the result of integer to floating point to integer conversion in the monitor.

At least one monitor (Dell 2407wfp) does not allow setting brightness less than 30.  **setvcp** values
in the range 0..50 are mapped to to 30..50. 

### ddcutil hangs on Radeon RX 5xxx and 6xxx cards (Navi and Navi2 GPU)

There have been multiple reports of **ddcutil** command failures on Radeon cards with the Navi and Navi2 GPUs 
(RX 5xxx and RX 6xxx cards).

In some cases **ddcutil** hangs.  (See for example issues  [Stop poking AMDGPU SMU I2C #194](ttps://github.com/rockowitz/ddcutil/issues/194), 
[Screen freeze when changing brightness #223](https://github.com/rockowitz/ddcutil/issues/223)).. 
The SMU related hang should have been fixed as of kernel 5.14.4.  **ddcutil** has had a workaround since release 1.1.0. 

In other cases, the monitor's EDID is simply not detected.

If your kernel version is 5.14.4 or later, I suggest you file a bug report at the [amd driver issue list](https://gitlab.freedesktop.org/drm/amd/-/issues).  

Include: 
- drm output 
- the kernel log file (/var/log/kernel on Ubuntu)
- output of the following commands
~~~
$ sudo ps -aux | grep ddcutil
$ sudo awk ' {print FILENAME ": " $0 } '  /sys/bus/i2c/devices/*/name
~~~

<a name="plasma_6"></a>
### KDE Plasma 6

ddcutil commands fail after upgrade to version 2.0 and KDE Plasma 6


ddcutil 2.0 added a cross-instance locking facility. The problem addressed was random failures when multiple ddcutil or libddcutil instances clobber each other's DDC transactions. Internally, ddcutil uses what are called display handles to represent /dev/i2c devices that have been opened at the OS level. Proper use of the API is, for each DDC transaction, to open a display, creating a display handle, perform the transaction, and then close it. This sounds expensive, but in fact the time spent opening and closing is negligible compared to the time spent performing the I2C operations of a DDC transaction.

When the cross-instance locking facility is enabled, and a display is opened creating display handle, ddcutil calls flock() on the underlying /dev/i2c device and holds the flock until the handle is closed. Unfortunately, at startup PowerDevil creates a display handle for each /dev/i2c device and never closes it. So the flock's are never released, blocking other ddcutil instances.

Cross-instance locking can be disabled using option --disable-cross-instance-locks. This option can be specified on the ddcutil command line, or in configuration file $HOME/.config/ddcutil/ddcutilrc. The latter is the only way to control libddcutil, which is used by PowerDevil. For a description of configuration files, see https://www.ddcutil.com/config_file.



### Powerdevil crashes repeatedly with **ddcutil** 2.1.0 

ddcutil 2.1.0 API introduced an initialization function, ***ddca2_init()***, into the libddcutil API
to give the client, e.g. PowerDevil, finer control of libddcutil's initialization. 
If the initialization function is not called, then default settings are used. There was a bug in default initialization that caused PowerDevil to crash repeatedly. This was fixed in ddcutil 2.1.2. Also, PowerDevil was changed to explicitly call the initialization function, thereby avoiding the bug. With two different fixes for same problem, it may not be obvious what change made PowerDevil work again. 

<a name="docker_container"></a>
### Does **ddcutil** work in containers?

User Nikolay Dandanov has reported success running ddcutil in a Docker container: 


I defined a new Arch Linux image for Docker using the following simple `Dockerfile`:
```Dockerfile
FROM archlinux:latest

RUN pacman --noconfirm -Syu && \
    pacman --noconfirm -S ddcutil && \
    rm -rf ~/.cache/*
```
 
I saved this to a file named ```Dockerfile``` and built a Docker image using
~~~docker build -t ddcutil:v0.0.1~~~.


I then ran the container and also mapped all `i2c` devices (otherwise, these were not available in the container):
```bash
docker run $(for dev in `ls /dev/i2c*`; do echo "--device $dev "; done) -it --name ddcutil ddcutil:v0.0.1
```

<!--
### setvcp of feature x60 (Input Source) fails with the message "Feature x60 does not support verifiation"

Use the ***--noverify*** option. The problem with trying to verify the changed value of feature x60 is that once the input device changes it's most likely that trying to read the current value from the original input device will fail. Therefore automatic verification does not take place for this feature.
-->

## Questions about building **ddcutil** 

<a name="missing_package"></a>
### ***configure*** complains that a required package does not exist, but it is installed on my system.

When building **ddcutil**, error messages from pkg-config (which is called by configure) can be misleading.
If configure complains that a package is not found but it seems to be installed, it's likely that what's 
missing is the associated development package (with a suffix like "-dev").
