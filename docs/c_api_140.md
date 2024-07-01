## Shared Library Changes for Release 1.4.0 

### Overview

This release contains a handful of changes, none of which affect backward compatibility. 
Consequently, the ***libddcutil*** SONAME is remains libddcutil.so.4.
The full shared library names is libddcutil.so.4.3.0. 
In conformance with Debian practice, the package name is ***libddcutil4***.

### API changes

Changed Function                    | Comments
------------------------------------|----------
ddca_set_sleep_multiplier           |&nbsp;allow 0 as valid value, return -1.0 for invalid value
ddca_set_default_sleep_multiplier   |&nbsp;allow 0 as valid value, return -1.0 for invalid value
ddca_enable_force_slave_address     |&nbsp;re-enabled
ddca_is_force_slave_address_enabled |&nbsp;re-enabled

<br>
Notes: 

- **ddca_enable_force_slave_address()** affects only the i2c-dev 
  write()/read() interface, not the ioctl() interface.
- The use of 0 as a sleep-multiplier value is very unlikely to work.
  It is permitted to allow for some unexpected API use.

### Miscellaneous 

- Write additional error and trace messages to system log.
  (Whether any messages are actually written to the system log is controlled by 
  **configure** options ***--enable-syslog***, ***--disable-syslog***. 
  The default is ***--enable-syslog***.