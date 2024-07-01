## Shared Library Changes for Release 2.1.4

Shared library **libddcutil** is backwardly compatible with the one in 
ddcutil 2.0.0. The SONAME is unchanged as libddcutil.so.5. The released library
file is libddcutil.so.5.1.2. 

Previously deprecated API function **ddca_create_display_ref()** (an alternative name for **ddca_get_display_ref()**) was removed in release 2.0.0. It has been restored, allowing some existing programs in Debian to use libddutil unchanged.
