
ddcutil and ddcui
=================

##Announcements

17 February 2024

**ddcutil** 2.1.4 restores deprecated API function **ddca_create_display_ref()** (an alternative name for **ddca_get_display_ref()**),
allowing some existing programs in Debian to use **libddutil** without changes.

13 February 2024

**ddcui** 0.5.4 contains a single change to ensure that required driver i2c-dev is loaded.

07 February 2024

**ddcutil** 2.1.3 fixes a significant bug in **libddcutil** that caused API calls to fail with status code
DDCRC_ARG (invalid argument) when passed an otherwise valid display reference for a display that doesn't support DDC/CI.

**ddcui** 0.5.3 checks to see that the **libddcutil** version is at least 2.1.3

For details about the changes and bug fixes, see [Release Notes](release_notes.md),
[Shared Library Changes for Release 2.1.3](c_api_213.md), 
and [ddcui Release Notes](ddcui_release_notes.md).
Changes are also summarized in the CHANGELOG.md files in the respective source trees.

27 January 2024

**ddcutil** 2.1.2 fixes a critical bug in **libddcutil** that caused older versions of PowerDevil to repeatedly crash and restart.
In addition, there are several minor fixes and changes to accommodate the proprietary Nvidia video driver.

**ddcui** 0.5.2 contains minor cleanup for release 0.5.2

For details about the changes and bug fixes, see [Release Notes](release_notes.md),
[Shared Library Changes for Release 2.1.2](c_api_212.md), 
and [ddcui Release Notes](http://www.ddcutil.com/ddcui_release_notes/).
Changes are also summarized in the CHANGELOG.md files in the respective source trees.

17 January 2024

**ddcutil** 2.1.0 incudes enhancements to further speed up initialization, and addresses some bugs in dynamic sleep adjustment.
The new cross-instance locking feature coordinates access to /dev/i2c devices by multiple ddcutil and libddcutil instances.
Several vaguely named options have been renamed to more clearly indicate their purpose.

**libddcutil5** can now watch for changes in connected monitors and for changes in DPMS state.  Clients register a callback
function that informs them of detected changes.

**ddcui** 0.5.0 makes use of the display change detection in **libddcutil** to remind the user to execute Action->Redetect Displays
when a change occurs.

For details about the changes and bug fixes, see [Release Notes](release_notes.md),
[Shared Library Changes for Release 2.1.0](c_api_210.md), 
and [ddcui Release Notes](http://www.ddcutil.com/ddcui_release_notes/).
Changes are also summarized in the CHANGELOG.md files in the respective source trees.

Michael Hamilton, developer of [VDU Controls](https://github.com/digitaltrails/vdu_controls), has once again
provided invaluable assistance in the development of this release, particularly in the design 
and implementation of display status change detection.  His project [ddcutil-service](https://github.com/digitaltrails/ddcutil-service)
packages **libddcutil** as a D-Bus service. 

Prebuilt packages for several Linux distributions and machine architectures can be found in 
the [usual locations](install.md).

Prior announcements can be found [here](prior_announcements.md).


23 October 2023

Release 1.4.5 extends release 1.4.1 with an additional **configure** command option, ***--enable-install-lib-only***.  If set, **make** builds and installs only shared library **libddcutil.so.4**.  This option may be useful for Linux distributions that must install this legacy library along with the newer **libddcutil.so.5** from **ddcutil** 2.0, in order to support existing applications that require that earlier library.
See [Notes for Linux Distribution Maintainers](mult_shared_libs.md).

27 September 2023

**ddcutil** 2.0.0 includes major changes in the areas of performance, usability, and reliability.
Highlights include: 

- Most users will find that command **ddcutil** just works "out of the box", without the need for manual configuation. 
Driver **i2c-dev** is loaded automatically in case it was not already built into the kernel.
When executing on a system running systemd (i.e. on almost every current Linux distribution) the logged on user automatically has read-write
access to the /dev/i2c devices associated with monitors, making excution as root or set up of group I2C
unnecessary. 

- The dynamic sleep algorithm was completely rewritten to use the minimal reliable sleep-multiplier
value.  Explicitly using option ***--sleep-multiplier*** to optimize performance should generally 
be unnecessary.

- The **libddcutil** API has been both extended and simplified. Because necessary changes
broke backward compatibility, the opportunity was taken for a major API cleanup.
Unneeded functions were removed, including many that had previously been deprecated. Most
client programs should build with minimal changes.  The new SONAME is **libddcutil.so.5**.
For backwards compatibility, **libddcutil.so.4** from **ddcutil** 1.4.1 should continue to be 
available as a separate package.

- **ddcui 0.4.0** uses the new features of the updated API. It requires **libddcutil.so.5**. 

For details about the changes and bug fixes, see [Release Notes](release_notes.md),
[Shared Library Changes for Release 2.0](c_api_200.md), 
and [ddcui Release Notes](http://www.ddcutil.com/ddcui_release_notes/).
Changes are also summarized in the CHANGELOG.md files in the respective source trees.

Michael Hamilton, developer of [VDU Controls](https://github.com/digitaltrails/vdu_controls), 
provided invaluable assistance in diagnosing many "edge cases", particularly when
monitors are turned off and on or go into and out of a DPMS sleep state. It's been a pleasure
to work with him.
I also want to acknowledge the several people who have been reverse engineering the
undocumented ways in which input switching is controlled on recent LG monitors.
Their work is summarized on this [wiki page](https://github.com/rockowitz/ddcutil/wiki/Switching-input-source-on-LG-monitors).
To facilitate sending commands to these monitors, option ***--i2c-source-addr*** was added.

Prebuilt packages for several Linux distributions and machine architectures can be found in 
the [usual locations](install.md).

Prior announcements can be found [here](prior_announcements.md).

##Introduction

**ddcutil** is a Linux program for managing monitor settings, such as brightness, color levels, and input source.
Generally speaking, any setting that can be changed by pressing buttons on the monitor can be modified by **ddcutil**.

**ddcutil** primarily uses DDC/CI (Display Data Channel Command Interface) to communicate with monitors implementing MCCS 
(Monitor Control Command Set) over I<sup>2</sup>C.  Normally, the video driver for the
monitor exposes the I<sup>2</sup>C channel as devices named /dev/i2c-n.  Alternatively, there is support for monitors (such as Eizo ColorEdge displays) that implement MCCS using a USB connection.  See [USB Connected Monitors](usb.md). 

A particular use case for **ddcutil**, and the one that inspired its development, is as part of color profile management. 
Monitor calibration is relative to the monitor color settings currently in effect, 
e.g. red gain.  ddcutil allows color related settings to be saved at the time 
a monitor is calibrated, and then restored when the calibration is applied.

Restrictions:  

- **ddcutil** does not support laptop displays, which are controlled using a special API, not I<sup>2</sup>C.   
- Generally speaking, **ddcutil** can be built in a virtual machine, but will not run in a VM.  This is
because the virtual video drivers do not implement I<sup>2</sup>C.  However, if the VM is connected to a separate
video card and is running a non-virtualized driver for the card in passthru mode, then **ddcutil** will work.  
- Nvidia's proprietary video driver may require special configuration.  See 
[Special Nvidia Driver Settings](nvidia.md).   
- Reading and writing ***Table*** type features is implemented but untested.  See [Table Features](table_features.md)  

**ddcutil** is released under the GNU Public License, V2 (GPLV2).
The source is hosted on [Github](https://github.com/rockowitz/ddcutil).

General support questions are best directed to the [issue tracker](https://github.com/rockowitz/ddcutil/issues) on Github.
Use of that forum allows everyone to benefit from individual questions and ideas. For details, see [Technical Support](tech_support.md). 

## ddcui

**ddcui** is a graphical user interface to **ddcutil**, built using Qt. For further information, 
see [ddcui Overview](ddcui_main.md).

**ddcui** is released under the GNU Public License, V2 (GPLV2).
The source is hosted on [Github](http://www.github.com/rockowitz/ddcui).

**ddcui** questions should also be directed the to the [ddcutil issue tracker](https://github.com/rockowitz/ddcutil/issues) on Github.


## Topics

Using **ddcutil**:

- [Command Overview](commands.md)
- [Command Output Examples](cmd_output.md)
- [Monitor Control Command Set](mccs_background.md)
- [Color Management](colormgt.md)

Commands and Options:

- [Detailed command and option documentation](command_detail.md)

Installation and Configuration:

- [Post-Installation Checklist](config_steps.md)
- [Install **ddcutil** From Prebuilt Packages](install.md)
- [Building From Source](building.md)
- [Configuration and Installation Diagnostics](config.md)

Other:

- [Frequently Asked Questions](faq.md)
- [Technical Support](tech_support.md)
- [Releases](releases.md)
- [Release Notes](release_notes.md)
- [Announcement History](prior_announcements.md)
- [Virtual Machines](virtual_machines.md)
- [Comparison with ddccontrol](ddccontrol.md)
- [Notes on Specific Monitors](monitor_notes.md)
- [DDC Null Response](ddc_null_response.md)
- [APIs](api_main.md)
- [Articles about ddcutil](articles.md)
- [References](bibliography.md)
- [Acknowledgements](acknowledgements.md)

## Packaging Status In Distribution Repositories

For packages maintained by **ddcutil**, see [Installing ddcutil and ddcui from Prebuilt Packages](install)

Those responsible for maintaining **ddcutil** related packages in Linux distributions should see
[Notes for Linux Distribution Maintainers](mult_shared_libs.md).

### ddcutil Packaging Status

<!--
[![Packaging status](https://repology.org/badge/vertical-allrepos/ddcutil.svg)](https://repology.org/project/ddcutil/versions)
-->

<a href="https://repology.org/project/ddcutil/versions">
    <img src="https://repology.org/badge/vertical-allrepos/ddcutil.svg" alt="ddcutil packaging status" columns=4 header="ddcutil packaging status">
</a>

### ddcui Packaging Status

<a href="https://repology.org/project/ddcui/versions">
    <img src="https://repology.org/badge/vertical-allrepos/ddcui.svg" alt="ddcui packaging status" columns=4 header="ddcui packaging status">
</a>

## Author

Sanford Rockowitz  <rockowitz@minsoft.com>
