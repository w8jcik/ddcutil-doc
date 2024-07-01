## Notes for the Next ddcui Release (Draft)

### Github Branch 0.4.0-dev

Requires libddcutil.so.5.x.x from ddcutil 2.0.0-dev or later.

**ddcui** now uses a separate configuration file, $HOME/.config/ddcutil/ddcuirc, 
instead of combining the [global] and [ddcui] sections of **ddcutil** configuration
file $HOME/.config/ddcutil/ddcutilrc. 

Added options:

- ***--syslog &lt;log level&gt;*** controls what messages are written to the system log 
by **ddcui** and **libddcutil**.
Valid levels are **NEVER**, **ERROR**, **WARNING**, **NOTICE**, **INFO**, and **DEBUG**.
The default is **NOTICE**.

- ***--noconfig***, ***--disable-config-file***. Do not obtain options from 
configuration file $HOME/.config/ddcutil/ddcuirc.


- ***--libopts "options"***. Passes an option string to the shared library.  This string is appended to 
the **libddcutil** option string obtained from the **ddcui** configuration file.

Several **ddcui** command line options existed solely to pass options to the shared library.  These can now be passed using 
option ***--libopts***. and have been eliminated: 

- ***--nousb***
- ***--trace***
- ***--trcfunc***
- ***--trcfile***

Building **ddcui**: 

cmake option DDCUTIL_PROJECT_DIR specifies the root directory of project **ddcutil**.  This allows for 
building **ddcui** using the latest development version of **libddcutil** without having to install it. 

e.g. 

~~~
cmake -D DDCUTIL_PROJECT_DIR=/my/ddcutil/project/dir
~~~

<!--  options --i1, --i2 -->