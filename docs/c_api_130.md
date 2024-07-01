## Shared Library Changes for Release 1.3.0

### Overview

This release contains a handful of changes, none of which affect backward compatibility. 
Consequently, the ***libddcutil*** SONAME (libddcutil.so.4) is unchanged.
The full library name is libddcuti.so.4.2.0
In conformance with Debian practice, the package name is ***libddcutil4***.


### API changes

New Function                          | Comments
--------------------------------------|----------
ddca_enable_sleep_suppression         |&nbsp;now a no-op, returns false
ddca_is_sleep_suppression_enabled     |&nbsp;always returns false
ddca_force_slave_address              |&nbsp;now a non-op, returns false         
ddca_is_force_slave_address_enabled   |&nbsp;always returns false  

<br>
Notes:

- Function **ddca_enable_sleep_suppression()** only eliminated sleeps in certain particular 
  situations where the DDC/CI specification is ambiguous.  The use of sleeps in these cases has proven unnecessary. 
- Function **ddca_force_slave_address()** had meaning only when using the i2c-dev write()/read() interface.
  **libddcutil** now uses the ioctl() interface exclusively.  (But note that use of the write()/read() interface
  was renabled in release 1.4.0.)


Changed Function             |  Comments
-----------------------------|-----------------------
ddca_report_active_displays  |&nbsp;show additional information

<br>
Notes:
ddca_report_active_displays() shows additional information about DRM connection, conflicting drivers.
<br>

### Miscellaneous Changes

- library initialization recognizes environment variable DDCUTIL_DEBUG_LIBINIT

