## Building From Source

### Building from tarball


**ddcutil** can be built from its [tarball](releases.md) in the usual way.
Unpack the tar file, change to its directory, and issue the commands: 
~~~
# autoreconf --force --install 
# ./configure
# make
# sudo make install
~~~

### **configure** options 

The **configure** script has several custom options.  Some reflect features under development, or tools used for development.
The publicly supported options are:

| Option                  | Default |      Purpose |
|------------------------|---------|-----|
| ***--enable-usb***     | yes     | Build with support for USB connected monitors.  Setting --disable-usb reduces size and package requirements. (Default is --enable-usb.) 
| ***--enable-drm***     | yes     | Build with libdrm.  Provides enhanced diagnostics, and is required for **libddcutil** to watch for hotplug events.  Setting --disable-drm reduces size and package requirements. (Default is --enable-drm.)
| ***--enable-lib***     | yes     | Build the shared library. (Default is --enable-lib.)
| ***--enable-asan***    | no      | Build for execution with the Google Address Space Sanitizer.
| ***--enable-envcmds*** | yes     | Controls whether commands **environment** and **usbenvironment** are included.
| ***--enable-udev***    | yes     | Controls whether udev is used. 
| ***--enable-syslog***  | yes     | Controls whether messages are written to the system log (Default is --enable-syslog.)
| ***--enable-install-lib-only*** | no | Build and install only the shared library.  Simplifies installation of an earlier version of libddcutil.

<!--
   | ***--enable-x11***     | yes     | Build using X11 services, used for enhanced diagnostics.  Setting --disable-x11 allows for use on systems lacking X11.  (Default is --enable-x11.)
-->
<!--
Notes:  
- Option ***--disable-envcmds*** forces ***--disable-drm*** and ***--disable-x11***   
- Option ***--enable-targetbsd*** forces ***--disable-envcmds***, ***--disable-udev***, and ***--disable-usb***
-->

Notes:  
- Option ***--disable-envcmds*** forces ***--disable-drm*** and ***--disable-x11***   


### Required packages

Because of the variation among distributions, only general guidelines can be 
given for ddcutil prerequisites.  Ultimately, you'll have to try to build ddcutil and see what breaks.

ddcutil requires the following packages for both building and execution (TO BE UPDATED):

- i2c-tools
- glib-2.0  (Note: glib-2.0 can be packaged with different names, e.g. libglib2.0-0, and may entail installing multiple packages.)
  Minimum version: 2.32
- libgudev  (e.g. libgudev1)
- libusb-1.0 (e.g. libusb-1.0-0) Minimum version 1.0.15 
- libudev (e.g. libudev1)
- libdrm2  Minimum version: 2.4.16
- libjansson Minimum version 2.0.0
- libxrandr2
- hwdata

On most platforms, development related files (e.g. headers files) are in separate packages having a 
"-dev" or similar suffix in their names.   ddcutil needs development packages for:

- libc6 (e.g. libc6-dev)
- glib2.0 (e.g. libglib2.0-0)
- libusb (e.g. libusb-1_0-devel, libusb-1.0-0-dev)
- udev or systemd  (udev may be part of systemd, e.g. libudev-devel, libudev-dev)
- xrandr (e.g. libxrandr-dev)
- libdrm2 (e.g. libdrm-devel)
- libjansson (e.g. libjansson-dev)

On older versions of Ubuntu, the i2c.h header file is found in a separate package.   
If the following package exists, it is required to build ddcutil.

- libi2c-dev 

However, as of libi2c 4.0, i2c.h is no longer part of this package.

Error messages from ***configure*** regarding missing packages can be misleading.  If ***configure*** complains that a package 
is not found but it seems to be installed, it's likely that the associated development package 
(with a suffix like "-dev") needs to be installed.

Building requires that the core build files be installed (e.g. the files listed in build-essential on Ubuntu) and also the pkg-config system. 

- pkg-config

Wnen **make install** is used, files are normally written to subdirectories of /usr/local.  Some distributions include these
subdirectories in the relevant paths, others do not.  In particular, it may be necessary to copy source file
/data/usr/lib/udev/rules.d/60-ddcutil.rules to directory /etc/udev/rules.d. 

### Building from git

The git repository for **ddcutil** is [here](http://www.github.com/rockowitz/ddcutil)

- Building **ddcutil** from git requires that the *autotools* related packages be installed.  
The exact packages vary from distribution to distribution.   On Ubuntu, these include:

    - autoconf, Minimum version: 2.50
    - automake, Minimum version: 1.13
    - autotools-dev  
    - libtool  
    - m4  

To configure the build, change to the main **ddcutil** directory and execute the file: 
~~~
# ./autogen.sh
~~~

Or issue the individual commands: 
~~~
# aclocal
# autoheader
# autoconf
# automake
~~~

Then to configure, build, and install **ddcutil**: 
~~~
# ./configure
# make
# sudo make install
~~~

### Shared library configuration

By default, command "**sudo make install**" installs the shared library files (**libddcutil.so.n.n** and symbolic links) in directory /usr/local/lib.
In order to be available, command **ldconfig** must be run and it must find the shared library file. "**sudo make install**"
automatically calls **ldconfig** after installing the library. For **ldconfig** to find the file, directory /usr/local/lib should be 
in a file in /etc/ld.so.conf.d.  This is the case in Ubuntu.

For further details, see: [Shared Library Configuration](shared_lib_config.md)

### Common issues:

- If you see a message "required file './ltmain.sh.' not found", run *libtoolize*
  (See https://www.gnu.org/software/automake/manual/html_node/Error-required-file-ltmain_002esh-not-found.html)  
- May get the following warning when running automake
~~~
src/Makefile.am:38: warning: compiling 'cmdline/cmd_parser_aux.c' in subdir requires 'AM_PROG_CC_C_O' in 'configure.ac'
~~~
This is an autotools versioning issue.  It appears that this warning can be ignored.  

TO DO: add notes on other warnings that can be ignored 


### Development configuration options. 

There are several **configure** options that exist for developing **ddcutil**.  They are listed here for completeness,
and should be left disabled.

- **--enable-callgraph** 
- **--enable-cffi**
- **--enable-cython**
- **--enable-doxygen**
- **--enable-failisim**
- **--enable-gobject-api**
- **--enable-swig**
- **--enable-testcases**

### Building **ddcui**

For notes on building **ddcui**, see [Building ddcui](building_ddcui.md)
