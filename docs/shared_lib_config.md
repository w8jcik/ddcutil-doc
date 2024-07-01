## Shared Library Configuration

When installation is performed using a package (e.g. rpm, deb), the include file, shared libraries, and pkgconfig file
are installed in the default system production locations.  On Fedora, for example, these are:
/usr/include, /usr/lib, and /usr/share/pkgconfig. If the package installation does not do it for you, it may be necessary to 
run **ldconfig** after installation to rebuild the shared library cache (normally file /etc/ld.so.conf) to make the shared libraries available.
~~~
$ sudo ldconfig
~~~

When installing from a .tar file, or building and installing from source, the situation is more complicated.  As the saying goes, your mileage may vary.

Include files, shared libraries, and pkg-config files are installed in distriibution specific local directories.
(This can be altered by the ***--prefix*** and other options to  **configure**, 
but that is beyond the scope of this discussion.  For details, see [Variables for Installation Directories](https://www.gnu.org/prep/standards/html_node/Directory-Variables.html).
For example, on Fedora these are **/usr/local/include**, **/usr/local/lib**, and 
**/usr/local/share/pkgconfig**. 

Depending on distribution, these local directories may or may not be part of the standard path
for the corresponding program.  For example, **/usr/local/lib** may or may not be automatically searched by **ldconfig**, 
**/usr/local/share/pkgconfig** may or may not be searched automatically by **pkg-config**, and the directory containing **ddcutil** include files may or
may not be part of the default include path. If they are, then as in the case of production locations, all that may be
necessary is to execute **ldconfig**. 

### Locating shared libraries

If the shared libraries are not found, the following command shows the library search path: 
~~~
$ ld --verbose | grep SEARCH_DIR | tr -s ' ;' \\012
~~~

To check if the linker can locate all the libraries required by a program, use the **ldd** command.  For example (assuming you're 
in the directory containing **ddcui**, which uses **libddcutil**)::
~~~
ldd ddcui
~~~

To add a directory to search, set environment variable **LD_LIBRARY_PATH** prior to executing a program that requires the shared library.
For example:
~~~
$ export LD_LIBRARY_PATH=shared_library_directory:$LD_LIBRARY_PATH
$ ./ddcui
~~~

To permanently add the local library directory to the search path, create a .conf file in directory **/etc/ld.conf.d** that specifies the local library directory, or if appropriate add the library directory name to an existing .conf file.

### Locating include files


To see the default include file path: 
~~~
$ gcc -xc++ -E -v 
~~~

If the local include directory is not in the default include path, you can add it to the default include directories, e.g.
~~~
$ export INCLUDE=/usr/local/include
~~~

Alternatively, you can rely on pkg-config to set the required directory at compile time.

###PKGCONFIG files

If **pkg-config** is called as part of building **Makefile**, e.g. from autoconf/automake, cmake, or qmake, then the ddcutil pkg-config file **ddcutil.pkg** specifies the required include files, libraries, and library directories.  

Now it's **ddcutil.pc** that must be found by **pkg-config**.  So the question becomes whether
the local directory in which ddcutil.pc is installed is in the distribution-specific default search path. 

To see the the default search path: 
~~~
$ pkg-config --variable pc_path pkg-config
~~~

To add a local directory to the **pkg-config** search path, set environment variable PKG_CONFIG_PATH.  Note that
this affects **pkg-config** when run by configure, cmake, qmake etc.  Examples:
~~~
$ export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
$ ./configure
~~~

