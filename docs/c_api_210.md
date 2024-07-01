<!--


    - ddca_enable_dynamic_sleep() 
    - ddca_disable_dynamic_sleep() 
    - ddca_get_current_sleep_multiplier() 
    - ddca_set_display_sleep_multiplier() 
  - ddca_init2() replaces ddca_init(), which is deprecated: 
    Has additional argument for collecting informational msgs. Allows for not
    issuing information messages regarding options assembly and parsing directly
    from libddcutil (currently enabled by setting flag DDCA_INIT_OPTIONS_ENABLE_INIT_MSGS),
    bus instead gives client complete control as to what to do with the messages.
  - ddca_stop_watch_displays(), ddca_start_watch_displays()
  - Cross-instance locking (experimental). Uses flock() to coordinate I2C bus
    access when multiple instances of libddcutil are executing. Enabled by option
    ***--enable-cross-instance-locks***.

#### Changed
- Functions that depend on initialization and that return a status code now 
  return DDCRC_UNINITIALIZED if ddca_init() failed.
- Revert ddca_get_sleep_multiplier(), ddca_set_sleep_multiplier() to 
  their pre 2.0 semantics changing the multiplier on the current thread.
  However, these functions are marked as deprecated.
- **configure** option ***--enable-x11*** is deprecated. ddcutil no longer
  makes any use of the X11 API. 

#### Fixed
- Argument passing on ddca_get_any_vcp_value_using_implicit_type()
- Fixed cause of assert() failure in ddca_init() when the libopts string
  argument has a value and the configuration file is enabled but 
  no options are obtained from the configuation file.
- Contents of the libopts arg were added twice to the string passed to 
  the libddcutil parser.
-->


## Shared Library Changes for Release 2.1.0

Shared library **libddcutil** is backwardly compatible with the one in 
ddcutil 2.0.0. The SONAME is unchanged as libddcutil.so.5. The released library
file is libddcutil.so.5.1.0. 


### Library Initialization

Changed                 |Comments
------------------------|----------------------
enum DDCA_Init_Options  |&nbsp; added value DDCA_INIT_OPTIONS_ENABLE_INIT_MSGS

<br>

New Functions                          |&nbsp;Comments
---------------------------------------|---------
ddca_init2                             |&nbsp;Replaces ddca_init

<br>

API function **ddca_init2()** replaces **ddca_init()**.  It has an an additiona parameter, **infomsg_loc**, 
for collecting informational msgs
    
This change givea the client control over what is done with informational 
messages issued by **libddcutil** at startup.  Previously, such messages were always written to the terminal.
Flag DDCA_INIT_OPTIONS_ENABLE_INIT_MSGS controls whether such messages are emitted.

With this change, DDCA_INIT_OPTIONS_ENABLE_INIT_MSGS still controls whether 
libddcutil writes messages to the terminal. If infomsg_loc is non-null, it specifies the 
addess at which to return a null-terminated array of messages.

Thus, to give the client complete control over what to do with the messages, 
do not set DDCA_INIT_OPTIONS_ENABLE_INIT_MSGS, so the server
does not write informational msgs, and use infomsg_loc to return
the informational msgs for display by the client.

For API functions that require libddcutil initialization, if ddca_init2() has not already been called, the function prolog automatically calls ddca_init2() with options such that it 
can never fail.  If an explicit call to ddca_init2() has failed, then any subsequent call to a function requiring initialization fails with 
status DDCRC_UNINITIALIZED


Changed Functions                  | Comments
-----------------------------------|-----------------------
ddca_init                          | &nbsp;Deprecated. Use ddca_init2() 

<br>

### Fixed

- segfault on freeing Parsed_Edid when command parsing failed



### Sleep multiplier control

New typedefs          |&nbsp;Comments
----------------------|----------
DDCS_Sleep_Multiplier | &nbsp;standardizes the type for sleep-multiplier values

<br>

Deprecated Functions               | Comments
-----------------------------------|----------
ddca_set_sleep_multiplier()        |&nbsp;
ddca_get_sleep_multiplier()        |&nbsp;

<br>


New Functions                             |Comments
------------------------------------------|--------
ddca_set_display_sleep_multiplier         |&nbsp;
ddca_get_current_display_sleep_multiplier |&nbsp;
ddca_enable_dynamic_sleep                 |&nbsp;
ddca_is_dynamic_sleep_eanbled             |&nbsp;

<br>

New Status Codes      |  Comments
-------------------------|----------- 
DDCRC_DISCONNECTED   |&nbsp;
DDCRC_DPMS_ASLEEP    |&nbsp;
<br>

### Display Hotplug Detection

libddcutil watches for display connection and disconnection, and for 
changes to DPMS state, and reports them to the client using callback functions.

Events can be connection or disconnection of a display, or change
in DPMS sleep state.  The effect of turning a monitor on or off is monitor dependant and 
    cannot reliably be detected. It is therefore not an event that is reported.

Callback functions must be of type **DDCA_Display_Status_Callback_Func**. 
The function delives a **DDCA_Display_Status_Event** to the client. 
The type of event being reported is specified by enum **DDCA_Event_Type**. 

New typedefs                         | Comments
-------------------------------------|----------
DDCA_Display_Status_Callback_Func |&nbsp; 

<br>

New enums               | Comments
------------------------|----------
DDCA_Display_Event_Type |&nbsp;type of display status event
<br>

New structs               | Comments
--------------------------|----------------
DDCA_Display_Status_Event |&nbsp;delived by callback function

<br>

New Functions                                | Comments
---------------------------------------------|---------
ddca_register_display_status_callback        |&nbsp;
ddca_unregister_display_status_callback      |&nbsp;
ddca_display_event_type_name                 |&nbsp;
ddca_start_watch_displays                    |&nbsp;
ddca_stop_watch_displays                     |&nbsp;
ddca_validate_display_ref  |                 |&nbsp;

<br>

Key points:
- When a event of type DDCA_EVENT_DISPLAY_DISCONNECTED is reported, 
the DDCA_Display_Reference specified in struct DDCA_Display_Status_Event
is no longer valid. 
- When an event of type DDCA_EVENT_DISPLAY_CONNECTED occurs, the client
must call **ddca_redetect_displays()** to reinitialize **libddcutil**
and the call **ddca_get_display_refs()** to get a new set of valid display references.
- Requires DRM video drivers (e.g. amdgpu, i915)
- A future release may provide a new valid display reference when a monitor is connected, 
making che call to **ddca_redetect_displays()** unnecessary.






    - new status codes possible for many current API functions: 
      DDCRC_DISCONNECTED, DDCRC_DPMS_ASLEEP

  - ddca_dref_state(): reports whether monitor asleep, disconnected, etc.
  - Sleep multiplier control:



Can report display connection/disconnection and dpms sleep status




<br>




<br>




<br>

### Miscellaneous

Changed                      |&nbsp; Comments
-----------------------------|----------
struct DDCA_Feature_Metadata | &nbsp;rename latest_sl_values -> unused




