

## Shared Library Changes for Release 2.1.2

Shared library libddcutil is backwardly compatible with the one in 
ddcutil 2.0.0. The SONAME is unchanged as libddcutil.so.5. The released library
file is libddcutil.so.5.1.0. 

It also fixes several minor bugs and has internal changes to better 
accomodate the proprietary Nvidia diver

Changed Functions                         | Comments
------------------------------------------|----------------------------------------
ddca_start_watch_displays               | &nbsp;Requires that all video drivers implement DRM
ddca_get_sleep_multiplier               | &nbsp;Revert to release 2.0.0 semantics
ddca_set_sleep_multiplier               | &nbsp;Revert to release 2.0.0 semantics

