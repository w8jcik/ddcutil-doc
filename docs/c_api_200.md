## Shared Library Changes for Release 2.0.0


### Overview

The shared library **libddcutil** is not backwardly compatible.
The SONAME is now libddcutil.so.5. The released library file is libddcutil.so.5.0.0.
In conformance with Debian practice, the package name is ***libddcutil5***.

To support programs that that use the old shared library, libddcutil.so.4 should continue
to be published as package **libddcutil4**.

Because necessary changes required incrementing the major version of the shared
library, it provided an opportunity to remove a large number of functions that 
were deprecated, made unnecessary by the ability to pass command line options
to **libddcutil**, or otherwise cluttered the API and ABI.  In practice, most
existing programs will require minimal changes to rebuild with the new version of the shared library.
However, best practice is to use initialization function **ddca_init()**.
It is required in order to read options from the configuration file, 

### Library Initialization

Library initialization has been reworked extensively to move operations 
that can fail, along with various settings that should be under control of the library user,
into a new function **ddca_init()**.

This function:

- controls the level of messages written to the system log  
- optionally processes options obtained from the **ddcutil** configuration file  
- processes additional options passed as a string  
- sets error information for ddca_get_error_detail()

If this function is not called by the user program, any API function that relies on its 
having executed invokes **ddca_init()** using arguments such that it never fails, e.g. 
the configuration file is not processed.

### Execution Options

These new options apply only to **libddcutil**, not to **ddcutil**.  They can be specified in configuration
file $HOME/.config/ddcutil/ddcutilrc, or in the libopts string passed to **ddca_init()**.

- Option ***--trcapi <api function name>***:  Trace the specified API function and its called functions.
- Option ***--profile-api***: Collect performance statistics for API functions that perform display IO, 
  and report them when the shared library is terminated.
    
### API changes

New typedefs      | Comments
------------------|----------
DDCA_Init_Options |&nbsp;enum of **ddca_init()** options
DDCA_Syslog_Level |&nbsp;enum of **ddcutil** severity levels for msgs written to the system log
DDCA_HotplugFunc  |&nbsp;signature of callback function reporting a display hotplug event

<br>

New Functions                          | Comments
---------------------------------------|---------
ddca_init                              |&nbsp;Perform library initialization
ddca_libddcutil_filename               |&nbsp;Returns the fully qualified name of the shared library
ddca_syslog_level_from_name            |&nbsp;Given its symbolic name, return a DDCA_Syslog_Level value
ddca_register_display_hotplug_callback |&nbsp;Registers a function of type DDCA_Display_Hotplug_Func which will be called to inform the client of display hotplug events.
ddca_unregister_display_hotplug_callback |&nbsp;Unregister a callback

<br>

Changed Functions                    | Comments
------------------------------------|----------
ddca_set_sleep_multiplier           |&nbsp;Operates on the display, if any, open in the current thread
ddca_get_sleep_multiplier           |&nbsp;Returns the value for the display, if any, open in the current thread
ddca_get_display_list               |&nbsp;Returns status code, sets error detail
ddca_report_display_info            |&nbsp;Returns DDCA_Status instead of void
ddca_show_stats                     |&nbsp;Optionally report per-display stats, not per-thread stats
ddca_get_feature_name()             |&nbsp;Implementation had been removed but the function remained in the API.  This change restores the implementation.
ddca-dfr_check_by_dref()            |&nbsp;Do not return an error if no user defined feature file exists
                                    

<br>

With the ability to configure libddcutil operation both by the ddcutil 
configuration file and by passing an option string in the **ddca_init()** arguments, 
many API functions are no longer needed and have been removed.
These are unlikely to have been used by any existing clients other than **ddcui**.


Removed Max-Tries Functions         | Equivalent Option
------------------------------------|-----------------------------------------
ddca_max_max_tries                  |&nbsp;No option equivalent
ddca_get_max_tries                  |&nbsp;No option equivalent
ddca_set_max_tries                  |&nbsp;Use option ***--maxtries***

<br>

Removed Sleep-Multiplier Functions  | Comments
------------------------------------|------------------------------------
ddca_set_default_sleep_multiplier   |&nbsp;Use option ***--sleep-multiplier***
ddca_get_default_sleep_multiplier   |&nbsp;No option equivalent
ddca_set_global_sleep_multiplier    |&nbsp;Previously deprecated
ddca_get_global_sleep_multiplier    |&nbsp;Previously deprecated

<br>

Removed USB Enablement Functions        | Comments
----------------------------------------|------------------------------------
ddca_enable_usb_display_detection       |&nbsp;Use options ***--enable-usb*** / ***--disable-usb***
ddca_is_usb_display_detection_enabled   |&nbsp;No option equivalent

<br>

Miscellaneous Removed Functions         | Comments
----------------------------------------|---------------
ddca_enable_report_ddc_errors           |&nbsp;Use option ***--ddc***
ddca_is_report_ddca_errors_enabled      |&nbsp;No option equivalent
ddca_enable_error_info                  |&nbsp;Use option ***--excp***
ddca_enable_force_slave_address         |&nbsp;Use option ***--force-slave-address***
ddca_is_force_slave_address_enabled     |&nbsp;No option equivalent

<br>

Removed Tracing Typedefs           | Comments
-----------------------------------|-----------------
DDCA_Trace_Options                 |&nbsp;used by ddca_set-trace_options() 

<br>

Removed Tracing Functions          | Comments
-----------------------------------|-----------------------------
ddca_add_traced_function           |&nbsp;Use option ***--trcfunc***
ddca_add_traced_file               |&nbsp;Use option ***--trcfile***
ddca_set_trace_groups              |&nbsp;Use option ***--trace***
ddca_add_trace_groups              |&nbsp;Use option ***--trace***
ddca_trace_group_name_to_value     |&nbsp;Not needed
ddca_set_trace_options             |&nbsp;Use options --thread-id --timestamp, --wall-timestamp

<br>

Most per-thread statistics are now instead maintained on a per-display basis.
The following functions are no longer useful and have been removed:

Removed Per-Thread Functions      | Comments
----------------------------------|--------------------
ddca_set_thread_description       |&nbsp;No longer useful
ddca_append_thread_description    |&nbsp;No longer useful
ddca_get_thread_desription        |&nbsp;No longer useful

<br>

Remove previously deprecated functions:

Removed Function              |&nbsp;Comments
------------------------------|--------------------
ddca_get_feature_name_by_dref |&nbsp;Had been replaced by ddca_get_feature_metadata_by_dref
ddca_get_formatted_vcp_value  |&nbsp;Did not support user defined features
ddca_open_display             |&nbsp;Had been replaced by ddca_open_display2
ddca_create_display_ref       |&nbsp;Use ddca_get_display_ref
ddca_free_display_ref         |&nbsp;Not needed. Display references are managed by the shared library.

<br>

Remove vestiages of the old AMD proprietary driver.
Regarding enum changes, while the constant numbers assigned to the remaining values values are 
unchanged, **switch** statements without a **default** case will fail to compile.


Changed                        |&nbsp;Comments
-------------------------------|----------------------------
enum DDCA_Build_Option_Flags   |&nbsp;Eliminated value DDCA_BUILT_WITH_ADL
struct DDCA_Adlno              |&nbsp;Eliminated
enum DDCA_IO_Mode              |&nbsp;Eliminated value DDCA_IO_ADL
struct DDCA_IO_PATH            |&nbsp;Removed variable adlno from path union

<br>

### Miscellaneous

Symbols for functions and enums that had previously been removed from ddcutil_c_api.h are no longer exported.
In addition, many internal symbols had "leaked".  These are no longer exported. 


