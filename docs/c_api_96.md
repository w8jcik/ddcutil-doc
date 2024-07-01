## C API Changes for Release 0.9.6

#### SONAME control

SONAME versioning has been enabled for **libddcutil**.  To differentiate the lastest
library from all prior libraries, the current version is 1.0.0, i.e. libddcutil.so.1.0.0. 


#### New Functions


| New Functions                          | Comment               |
|----------------------------------------|-----------------------|
| ddca_enable_usb_display_detection      |&nbsp; Control whether USB display detection is enabled |
| ddca_is_usb_display_detection_enabled  |&nbsp; Indicates if USB display detection is enabled |

Notes on ddca_enable_display_detection():

- Must be called before any function call that triggers display detection. 
- Returns **DDCRC_INVALID_OPERATION**. if display detection has already occurred.
- Returns **DDCRC_UNIMPLEMENTED** if **ddcutil** was built without USB support.
- This setting is global to all threads.

#### Changed functions 


**ddca_get_feature_list_by_dref()**

No longer returns **DDCRC_ARG** to indicate that the VCP version was unqueried, as this error is no longer possible.
**DDCRC_ARG** now always indicates an invalid display reference.

**ddca_open_display2()**

New status code **DDCRC_ALREADY_OPEN** indicates that the display is already open in the current thread.  (As before, **DDCRC_LOCKED** indicates the display is open in another thread.)


### Sample Program Changes

Fix the cause of a failure in **demo_feature_list.c**. 

