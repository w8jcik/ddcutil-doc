## Prior Announcements


15 June 2023

Release candidates **ddcutil 2.0.0-rc1** and **ddcui 0.4.0-rc1** are now available for testing 
as branches with those names on the github [ddcutil](https://github.com/rockowitz/ddcutil) 
and [ddcui](https://github.com/rockowitz/ddcui) repositories.

**ddcutil** 2.0.0 is a major release.  It provides significant performance improvements
by caching information queried from displays across program invocations, and by dynamically
adjusting the sleep-multiplier time. It also should work "out of the box",
without the need for post-installation configuration.

The **libddcutil** API has been both extended and simplified. Because certain important
changes broke backward compatibility, I took the opportunity for a major API cleanup.
Unneeded functions were removed, including many that were previously deprecated. Most
client programs should build with minimal changes.  The new SONAME is **libddcutil.so.5**.
For backwards compatibility, **libddcutil.so.4** from **ddcutil** 1.4.1 will continue to be 
available as a separate package.

**ddcui 0.4.0-rc1** is built to work with the updated API in ddcutil 2.0.0-rc1.

Preliminary release notes are available at [Notes for the Next ddcutil Release (Draft)](http://www.ddcutil.com/next_release_notes/),
[Shared Library Changes for the Next Release (Draft)](c_api_200.md),
and [Notes for the Next ddcui Release (Draft)](http://www.ddcutil.com/ddcui_next_release_notes/).
Changes are also summarized in the CHANGELOG.md files in the respective source trees.

17 January 2023

**ddcutil** release 1.4.1 fixes a critical bug in release 1.4.0.  The default sleep-multiplier value was 0 instead of 1.  As a result,
commands that did not have an explcit ***--sleep-multiplier*** option would fail.  All users of release 1.4.0 should upgrade.
Thanks to github users copysiper (Oleg Tsvetkov) and fionnb for their help in quickly identifying and resolving this bug.


13 January 2023

**ddcutil** release 1.4.0 contains the following changes of general interest:

- **ddcutil** installation now creates a udev rule file that gives the logged on user read/write access to I<sup>2</sup>C 
  devices associated with video monitors. Use of group **i2c** for granting permission to non-root users is,
  in general, no longer needed.
- Use of driver i2c-dev's file io interface (read()/write()), removed in release 1.3.0, has been restored. 
  When i2c-dev's ioctl() interface is used the the Nvidia proprietary driver, depending on how the 
  driver was built, it is possible for display detection to fail. If the Nvidia failure is detected, **ddcutil** switches
  to using the read()/write() interface for the Nvidia driver.
  In this case option ***--force-slave-address*** may be needed to handle EBUSY errors.
  For all other video drivers, the ioctl() interface is used and ***--force-slave-address*** has no effect.

For a detailed description of **ddcutil** changes and bug fixes, see [Release Notes](release_notes.md).


27 September 2022

Depending on how it was built, the proprietary nvidia driver can be incompatible with **ddcutil** 1.3.0 and 1.3.2.
The effect is that no monitors are detected. Users encountering this problem should revert to using release 1.2.2, 
or can use the latest development branch on github.  For details, see this github [issue](https://github.com/rockowitz/ddcutil/issues/282).
Thank you to github user [KeyOfBlueS](https://github.com/KeyofBlueS) for extensive and expert help in reporting and diagnosing this problem.

04 September 2022

**ddcutil** release 1.3.2 modifies tarball creation to eliminate garbage and otherwise unneeded files.
Users of release 1.3.0 need not upgrade.

05 August 2022

**ddcui** release 0.3.0 contains the following changes of interest to general users;

- CTL-Q terminates **ddcui** (does not apply within dialog boxes)
- Errors opening /dev/i2c and /dev/usb/hiddev devices are reported using a message box
  instead of being written to the terminal. These errors typically  reflect lack of permissions (Linux error EACCESS).
- Optionally requiring the control key to be pressed when changing feature values now applies
  to all changes, not just those made using sliders. (The option is set using command line
  option ***--require-control-key*** or the UI Options dialog box.).
- Options ***--force-slave-address*** and ***--disable-force-slave-address***, introduced in Release 0.2.1, are deprecated and no longer have any effect.

For a detailed description of **ddcui** changes and bug fixes in this release, see [ddcui Release Notes](ddcui_release_notes.md).

Prebuilt packages for several Linux distributions and mmachine architectures can be found in 
the [ususal locations](install.md).

Prior announcements can be found [here](prior_announcements.md).



23 July 2022

**ddcutil** release 1.3.0 contains the following changes of interest to general users: 

- Option ***--force-slave-address*** no longer has any effect.
Driver i2c-dev provides both a low level ioctl() interface and a higher level read()/write() 
interface for I<sup>2</sup> communication.  **ddcutil** now uses the ioctl() interface (almost) exclusively. 
This eliminates the possbility of EBUSY errors from driver i2c-dev, which occur only when the read()/write() interface is used.
- Better reporting of user configuration issues at startup.
- Commands **getvcp** and **vcpinfo** can now take multiple features as arguments, for example **ddcutil getvcp 10 12**.
- The sleep-suppression related options, ***--sleep-less***, ***--less-sleep***, ***--enable-sleep-suppression***,
  and ***--disable-sleep-suppression*** no longer have any effect.

For a detailed description of **ddcutil** changes and bug fixes, see [Release Notes](release_notes.md).

Prebuilt packages for several Linux distributions and mmachine architectures can be found in 
the [ususal locations](install.md).

14 June 2022

**ddcui** 0.2.1 (along with **ddcutil** 1.2.2) is now available in the Debain Testing distribution (Bookworm). 


02 February 2022

**ddcui** release 0.2.1 better handles DDC communication failures resulting from conflict with other programs.
Like **dddcutil** 1.2.2, **ddcui** command line option ***-force-slave-address*** allows **ddcui** to override the other program's
claim to the display.

For a detailed description of **ddcui** changes and bug fixes in this release, see [ddcui Release Notes](ddcui_release_notes.md).

28 January 2022

**ddcutil** release 1.2.2 contains one set of changes of interest to general users: 

- Handling of DDC communication failures due to conflict with other programs is improved.
In particular, conflict occurs when device driver ddcci is loaded. Among the changes:
  - If **ddcutil detect** fails to communicate with a monitor and Linux errno EBUSY is reported, 
  it reports the monitor as "Busy" instead of "Invalid" and suggests the use of option ***--force-slave-address***. 
  - Messages regarding EBUSY errors are always written to the system log.
  - API functions **ddca_enable_force_slave_address()** and **ddca_is_force_slave_address_enabled()** have been added.

For a detailed description of **ddcutil** changes and bug fixes, see [Release Notes](release_notes.md).

02 December 2021

**ddcutil** release 1.2.1 contains one change of interest to general users: 

- Option ***--settings***. Details of current settings are no longer reported by every command invocation when option ***--verbose*** is specified.  Use option ***--settings*** to control option reporting.

For a detailed description of changes and bug fixes, see [Release Notes](release_notes.md).

25 November 2021

**ddcui** release 0.2.0 is is now available on [Github](https://github.com/rockowitz/ddcui), and also from this site as a [tarball](./tarballs/ddcui-0.2.0.tar.gz).
While not feature complete, this 
version should be useful to and usable by most people. For details, see [**ddcui** Release Notes](ddcui_release_notes).


24 November 2021

**ddcutil** release candidate 1.2.1-rc2 is available as [branch 1.2.1-rc2](https://github.com/rockowitz/ddcutil/tree/1.2.1-rc2) on Github.
For details, see [the announcement](https://github.com/rockowitz/ddcutil/issues/235) on Github, file CHANGELOG.md, or the draft
[release notes](release_notes).

05 October 2021

**ddcutil** release 1.2.0 contains the following changes of general interest: 

- Command **ddcutil --verbose** now reports the raw EDID.
- Command **ddcutil environment --verbose** has been enhanced, primarily for remote /sys analysis.
- Major events are written to the system log.

Most visible changes apply to the shared library.  In particular:

- The shared library name is now libddcutil.so.4.1.0.
- Improved tracing.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).

Prior announcements can be found [here](prior_announcements.md).

05 April 2021

**ddcutil** release 1.1.0 contains the following changes of general interest:

- Configuration file **ddcutilrc**, located on the XDG config path (normally $HOME/.config/ddcutil/ddcutilrc)  is processed by both **ddcutil** and **libddcutil**. 
- Monitor capabilities strings are cached to improve performance of the **capabilities** command.
- Partial workarounds for problems in DRM video drivers (e.g. i915, AMDGPU) when monitors are connected to a docking station. The monitor may not be detected (because no EDID is reported), or the monitor may appear as two different /dev/i2c devices. 
- When probing /dev/i2c devices for monitors, **ddcutil** could put AMD Navi2 devices (e.g.  RX6000 series) into an inconsistent state. This is because the driver exposes an I<sup>2</sup> device for the SMU.  Display detection is modified to avoid probing these devices. 

The interface exposed by shared library **libddcutil** is unchanged. Its SONAME remains "libddcutil.so.4", and the package name is **libddcutil4**.

For a complete list of **ddcutil** changes and bug fixes, see [Release Notes](release_notes.md).
<!--
For **ddcui** changes and bug fixes, see [ddcui Release Notes](ddcui_release_notes.md).
-->
Prior announcements can be found [here](prior_announcements.md).

08 February 2021

**ddcutil** release 1.0.1 addresses the situation wherein a display connected through a docking station is not detected 
when the i915 video driver is used.  (I<sup>2</sup> support for docking station connected displays, and more generally displays using 
DisplayPort Multi Stream Transport. was recently implemented for i915 and amdgpu drivers in Kernel 5.10. Initial **ddcutil**
code for this situation did not adequately work around problems in the driver implementation.

03 February 2021

**ddcutil** release 1.0.0 marks a milestone.  It is time to regard **ddcutil** as having reached production status.  There of course remain features to add, and the codebase warrants cleanup, but the feature set has been stable for some time. Bug reports are infrequent, and reflect unusual situations. 

There are two enhancements of general interest:

- [User Defined Features](udf.md), which have existed for some time, are now formally available.  
- Option ***--mccs*** now applies to **getvcp**, **setvcp**, and **dumpvcp** as well as **vcpinfo**. 

The remaining changes in this release, while extensive, are likely of little interest to most users.  The most notable changes are:

- ***ddcutil*** no longer by default checks for monitors exposing the Virtual Control Panel over USB.  Such monitors have proven rare, and messages about lack of permission to read /dev/usb/hiddev devices can be be needless confusing.  Command option ***--enable-usb*** will activate handling of USB connected monitors.  
- The **detect** commands provides additional information about the monitors found.  
- The **capabilities** command better handles malformed capabilities strings.  
- **getvcp** has special handling for features x62 (audio volume), x87 (audio treble), x91 (audio bass), and x93 (audio balance). In MCCS 2.2 and 3.0, these otherwise continuous VCP 
features have special reserved values.  
- Parsing **setvcp** command arguments was incorrect when multiple features are changed on a single **setvcp** command.  

The shared library API has minor, but not upwardly compatible, changes.  The current library SONAME is libddcutil.so.4, and the library package is libddcutil4. 

Prior announcements can be found [here](prior_announcements.md).


09 July 2020

Branch master on github was out of sync with the [reference tarball](tarballs/ddcutil-0.9.9.tar.gz) and prebuilt packages on [launchpad](https://launchpad.net/~rockowitz/+archive/ubuntu/ddcutil),
[COPR](https://copr.fedorainfracloud.org/coprs/rockowitz/ddcutil/), and the [openSUSE Build Service](http://download.opensuse.org/repositories/home:/rockowitz/). The missing changes have been pushed to the master branch. If you have already built release 0.9.9 from the github master branch, pull the changes and rebuild.

28 June 2020

**ddcui** release 0.1.2 is an emergency bug fix release.

- Combo boxes for simple Non-Continous features were empty instead of showing the currently selected value and list of allowed values.

24 June 2020 

**ddcutil** release 0.9.9 contains two changes of general interest: 

- A new command line option, ***--less-sleep*** eliminates many of the delays between the time that **ddcutil** receives a DDC response from a display
and sends the next request, improving performance.
<!-- - User Defined Features are now supported. -->
- Support for AMD's old proprietary **fglrx** driver has been dropped.

The **libddcutil** shared libray has non-upwardly compatible changes.  For details, see [Shared Library Changes for Release 0.9.9](c_api_99.md).

**ddcui** release 0.1.1 adds additional features and has numerous changes in the "fit and finish" category.  In particular:

- Command line options for starting ***ddcui***, corresponding to settings in the ***Option*** dialogs, and (if multiple monitors) 
  the initlal monitor selected.
- The ***Feature Selection Dialog***, along with command line option ***--custom-feature-set***, 
enable the user to specify the VCP features to be shown.
- Better handling of error conditions
- Better **Help** text

**ddcui** release 0.1.1 requires ddcutil 0.9.9.

For general information about the graphical user interface, see [ddcui](ddcui_main.md).

For a complete list of **ddcutil** changes and bug fixes, see [Release Notes](release_notes.md).  
For **ddcui** changes and bug fixes, see [ddcui Release Notes](ddcui_release_notes.md).  
Prior announcements can be found [here](prior_announcements.md).

Lastly, this website has been extensively revised to reflect the current state of **ddcutil** and **ddcui**.

**Note:** Prebuilt packages are now available for **ddcutil** 0.9.9.  

06 December 2019

***ddcutil*** release 0.9.8 contains two changes of general interest:  
- A new command line option, ***--sleep-multiplier***, adjusts the time **ddcutil** pauses between sending a request to the monitor
and reading from the monitor.   
- A fix for the bug that certain mice would lock up during display detection. 

The **libddcutil** shared library API has some minor extensions.  For details, see [Library Changes for Release 0.9.8](c_api_98.md).

Graphical user interface **ddcui** has reached beta status.  Release 0.1.0 contains numermous improvements in the handling of particular VCP feature codes and
the user interface generally.  Prebuilt packages now exist for many platforms. See [ddcui Overview](ddcui_main.md).

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

08 October 2019

**ddcui** alpha release 0.0.6 is now available on [github](https://github.com/rockowitz/ddcui).  It requires the version of **libddcutil** provided by **ddcutil** release 0.9.7.

04 September 2019

**ddcutil** release 0.9.7 fixes the cause of a segfault during display detection if a monitor appearing to support 
the **USB Monitor Control Class Specification**, i.e. one that uses USB to communicate monitor settings, is connected to the system.
If also contains some minor code cleanup. 

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

25 August 2019

**ddcutil** release 0.9.6 contains minor enhancements, bug fixes, and API changes.

SONAME versioning is now enabled for **libddcutil**.  

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

24 February 2019

**ddcutil** release 0.9.5 contains minor enhancements, bug fixes, and API changes.

Of particular note:  
- Support for feature x72 (gamma) has been added.  
- Command ***setvcp*** can now take 2 byte values (0..65535) as an argument.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

**ddcui** release 0.0.4 reflects changes to the **ddcutil** API. 

25 December 2018

**ddcutil** release 0.9.4 fixes a bug that caused the ***capabilities*** command to fail, and addresses the
failure to build from source for the [x32 ABI](https://en.wikipedia.org/wiki/x32_ABI).

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

26 November 2018

**ddcutil** release 0.9.3 reworks display detection to avoid problems encountered with certain recent displays and laptops.
There have been extensive internal changes to allow for future support of user supplied definitions for manufacturer-specific
features.  The API has also been revised to support user supplied feature definitions (see [API Changes in Release 0.9.3](c_api_93.md)).

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

**ddcui** release 0.0.3 reflects changes to the **ddcutil** API. 

15 September 2018

**ddcui** is a Qt-based graphical user interface for **ddcutil**.  An initial alpha release is now available
on [github](https://github.com/rockowitz/ddcui).  **ddcui** must currently be built from source - there are no
pre-built (e.g. dpkg or rpm) packages.  For addtional information and rudimentary build instructions,
see files [README.md](https://github.com/rockowitz/ddcui/blob/master/README.md) and
[BUILDING.md](https://github.com/rockowitz/ddcui/blob/master/BUILDING.md).

01 September 2018

Release 0.9.2 contains minor enhancements and bug fixes.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

27 May 2018

Release 0.9.1 fixes the cause of a failure that may occur on command ***environment --verbose*** when examining an extremely large log file.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

13 May 2018

Release 0.9.0 contains minor enhancements and a few very minor bug fixes.  However. the C API is extensively revised
(see [API Changes in Release 0.9.0](c_api_90.md)).

Two enhancements will be of interest to general users:  
- The **setvcp** command now allows new values for Continuous type features to be specified as relative values,
e.g. the following commands increase or decrease the value of the brightness feature by 5. 
~~~
$ ddcutil setvcp 10 + 5
$ ddcutil setvcp 10 - 5
~~~
Note that parsing requires that "+" and "-" be surrounded by spaces.

- Table type features are by default not included in most feature groups specified on **getvcp**, e.g. ***getvcp known***.
Features of type Table are rare.  The DDC/CI spec does not provide a clean way for **getvcp** to determine that a table 
feature does not exist.  As a result, **getvcp** typically has to exceed its retry count before giving up.
(Exclusion of table features does not occur if a feature is explicitly specified by its hex code, or for feature group ***TABLE***.)  
The ***--no-table*** (formerly ***--notable***) and ***-show-table*** options explicitly control this behavior.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

20 January 2018

Release 0.8.6 contains a few minor externally visible enhancements and bug fixes, along with non-upwardly
compatible changes to the C API.  Most current users will see no need to upgrade.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

**ddcutil** is now in the repositories for the upcoming Ubuntu 18.04 release and the openSUSE Tumbleweed rolling release. 

16 November 2017

Release 0.8.5 contains a large number of minor enhancements and bug fixes.  Users of **ddcutil** on 32 bit platforms and
those on the Raspberry Pi should upgrade.  

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior announcements can be found [here](prior_announcements.md).

15 October 2017

Package **ddcutil** has been sponsored into Debian and is now included in [Debian Testing](https://wiki.debian.org/DebianTesting).
It is on track to be part of the next Debian release, which means it should eventually appear in downstream distributions such as Ubuntu. 
Note that only package **ddcutil**, containing the command line version of **ddcutil**, is currently in the Debian repositories.
The shared library packages, including the C and Python APIs, are not yet part of Debian.

22 July 2017

Release 0.8.4 primarily contains packaging changes to meet Fedora distribution standards.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).

17 May 2017

Release 0.8.2 contains minor enhancements, primarily to diagnostics in the **environment** and **interrogate** commands. 

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).

05 May 2017

Release 0.8.1 is a bugfix release that addresses a segfault that can occur when scanning for 
USB connected monitors.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).

01 May 2017

Release 0.8.0 contains new features intended to address issues with particular monitors and user environments,
and to improve performance

The most significant changes visible to users are:  

- If there are multiple monitors and option **--async** is specified, initial monitor checks are performed in separate threads.
Users with multiple monitors should see significantly better startup time.  
- If a display is specified by its I2C bus number (option **--bus**) and option **--nodetect** is specified,
global display detection is skipped, improving performance.
- By default, **setvcp** and **loadvcp** now read the VCP value after it has been set, 
to confirm that the monitor has made the change requested.  
- Command **getvcp --terse** now reports VCP settings in a form that is easily machine readable.
- The C API has been extensively revised.  Many names have changed for consistency and clarity.  
(Apologies to those of you who have written applications.)

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).


### OBS

29 April 2017

At some time in the recent past, the files for release 0.7.3 in the OpenSUSE Build Service were corrupted 
with files from the test system.  Symptoms of this corruption include

- Prefixing a VCP feature code number with "0x" causes a segfault, e.g. "ddcutil setvcp 0x10 50"  
- Command line option "--verify" is recognized

As of this morning, the production ddcutil project on OBS was restored to release 0.7.3.


### 0.7.3

05 March 2017

Release 0.7.3 is a maintenance release.  There is no need to install it unless you are experiencing problems.

This release introduces the following features:

- Command **ddcutil detect --verbose** shows additional information.
- Command **ddcutil environment --verbose** implements additional diagnostic tests.
- **configure** option ***--enable-drm*** controls whether DRM is used to enhance the diagnostics of command **ddcutil environment --verbose**.
If ***enable-drm=yes***, package libdrm-dev or libdrm-devel (depending on Linux distribution) must be installed to build **ddcutil**.
Setting ***--enable-drm=no*** affects only diagnostic output, not normal execution.

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).


### 0.7.2

01 February 2017

Maintenance release.

- Fixes a critical bug in release 0.7.1 where insufficient privileges on a /dev/i2c-n device causes program termination.
- **ddcutil interrogate** always executes with ***--set-slave-address*** in effect
- Minor improvements to **ddcutil environment** and **ddcutil interrogate** output
- Minor improvements to **ddcutil detect --verbose**
- Add explanations for additional ***errno*** values

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).


### 0.7.1

27 January 2017

Release 0.7.1 is a maintenance release.  There is no need to install it unless you are experiencing problems.

This release introduces the following features:

- Command option ***--force-slave-address*** causes **ddcutil** to attempt to take control of slave addresses
on the I<sup>2</sup> bus even if they are in use by another driver.  This may aid in certain situations where monitors on 
an I<sup>2</sup> bus are not properly detected. See [Instrumentation and Tuning](other_options.md#force_slave))

For a complete list of changes and bug fixes, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).

### 0.7.0

03 January 2017

Release 0.7 introduces the following features: 

- C API, exposed by shared library **libddcutil**.  See [API](api_main.md).  
- A new command line option ***--mfg***, allows for the 3 character manufaturer id found in the EDID to used as part of monitor selection.
- The monitor feature and capabilities portion of ***interrogate*** is also exposed by the  new ***probe*** command which explores the capabilities string and features found on a single monitor.

For details, see [Release Notes](release_notes.md).
Prior annoucnements can be found [here](prior_announcements.md).

openSUSE Build Service packaging of release 0.7.0 is complete.  Owing to Launchpad constraints, the PPA will not be correct until **ddcutil**'s release is incremented, i.e. until release 0.7.1

### 0.6.1

21 November 2016

The most recent release of **ddcutil** is 0.6.1.  This is a maintenance release and need not be installed
unless you are experiencing problems.  For details, see [Release Notes](release_notes.md).

### 0.6.0

01 October 2016

As of release 0.6, the name of this program changed from **ddctool** to **ddcutil** 
to avoid confusion with a commerical datacenter program also named **ddctool.**

The following are affected:

|Object            |Old name                             | New Name                             | Comments|
|-------------------|--------------------------------------|--------------------------------------|
|Repository URL     |https://github.com/rockowitz/ddctool.git | https://github.com/rockowitz/ddcutil.git | Old name still works |
|Web site URL       | http://www.ddctool.com               | http://www.ddcutil.com |  Old name still works |
|Primary executable | ddctool                              | ddcutil |
|Shared library     | libddctool.so                        | libddcutil.so |
