## ddcui Overview

**ddcui** is a graphical user user interface for **ddcutil**, implemented using Qt.  While still 
considered to be in development, the code has been stable for some time. 

### Views

The application has 3 views: **Summary**, **Capabilities**, and **Features**

The **Summary View** for a monitor reports basic monitor information,
similar to the output shown by command **ddcutil detect --verbose**. 
The view exists for every monitor detected, whether or not it supports DDC/CI.

![ddcui_summary](/screenshots/ddcui_summary.png)

The **Capabilities View** for a monitor reports its raw capabilities string, and also
a parsed interpretation based on the Monitor Control Command Set (MCCS) specification. 
This is comparable to the output shown by command **ddcutil capabilities --verbose**.
The view exists only for monitors that support DDC/CI. 

![ddcui_capabilities](screenshots/ddcui_capabilities.png)

The **Features View** for a monitor reports the value of every monitor feature.
Modifyable feature values can also be changed.
The view exists only for monitors that support DDC/CI. 

![ddcui_features](/screenshots/ddcui_features.png "Features View")

### Source code and prebuilt packages:   
 
- Code is available on [GitHub](https://github.com/rockowitz/ddcui).  Some prebuilt packages are available on 
[Copr](https://copr.fedorainfracloud.org/coprs/rockowitz/ddcutil/) and 
[Launchpad](https://launchpad.net/~rockowitz/+archive/ubuntu/ddcutil)  
- The latest **ddcui** release is [0.4.0](https:/github.com/rockowitz/ddcui/releases/tag/v0.4.0), 
It requires **ddcutil** [release 2.0.0](https://github.com/rockowitz/ddcutil/eleases/tag/v2.0.0) or later.

For instructions on how to build ***ddcui***, see [Building **ddcui**](building_ddcui.md). 

<!--
Both a CMake ***CMakeLists.txt*** and a Qt ***ddcui.pro*** file are provided.
-->


Prebuilt packages can be found on:   

- [launchpad](https::/launchpad.net/~rockowitz/) for Ubuntu, Debian, and derivatives. 
Note that on [launchpad](https://launchpad.net) **ddcui** and **ddcutil** are found together.  
- [copr](https://copr.fedorainfracloud.org/coprs/rockowitz/ddcutil/) for Fedora, openSUSE, Centos.
Note that on [copr](https://copr/fedorainfracloud) **ddcui** and **ddcutil** are found together.

### Unexpected Behavior

#### Incorrect values shown

It is possible for feature values reported in **ddcui** to become out of sync with actual monitor 
values.

- If feature values are changed using the monitor's On Screen Display.  
- If feature values are changed by another program, including the command line program **ddcutil**. 
- Some monitors will change their state (e.g. red gain) when the value in the GUI changes.
However, the monitor still reports the old value, which is shown in **ddcui**.  This is a bug 
in the monitor's DDC/CI implementation.  
- Conversely, some monitors will report a newly set value, but the observed state of the monitor
is unchanged. Again, this is a bug in the monitor's DDC/CI implementation.

In some, but not all, cases, **ddcui**
can be resynced with the actual monitor settings using menu item Actions-&gt;Rescan Monitor Values

#### Values do not change

If values for features such as contrast cannot be changed, it is possible that the monitor is in
a mode such as Cinema that disables changes. Check the on-screen display.  It may be simplest just
to reset the monitor to factory defaults.

#### Accidentally changing a feature value 

- When swiping the mouse or using the mouse wheel to scroll the VCP Features section, it is  
possible to accidentally move a slider
that changes a continuous feature value.  Optionally, the UI behavior can be 
altered to require that the control key be held down in order to move change values.
This behavior is modified using dialog ***Options->User Interface Options->Require control key to move sliders***.

Pages with additional **ddcui** information:   
- [Command line options](ddcui_options.md)   
- [Building **ddcui**](building_ddcui.md)  
- [Prebuilt Packages](install.md)
- [**ddcui** Release Notes](ddcui_release_notes.md)
