## C API Changes for Release 0.9.5

### Display Reference Lifecycle

While transient display references can exist internally in the command line version of ddcutil,
in the shared library all display references are persistent.  They are created during
display enumeration at startup.  Function **ddca_create_display_ref()** has been renamed
to **ddca_get_display_ref()** to better indicate that it returns an existing display reference.
There is never a need to free a display reference.

| Function                  | Comment               |
|---------------------------|-----------------------|
| ddca_get_display_ref()    |&nbsp; Renamed from ddca_create_display_ref() |
| ddca_create_display_ref() |&nbsp; Marked as deprecated  |
| ddca_free_display_ref()   |&nbsp; Marked as deprecated, unnecessary | 

<br>

Thank you to J&uuml;rgen Altfeld for his questions about this and other aspects of the API
that have helped make it more consistent and hopefully easier to use.



### Thread Error Detail

All API functions that return a **DDCA_Status** value now clear the thread error detail during function initialization.  Therefore, a call to
**ddca_get_error_detail()** is guaranteed to either return detailed error information for the immediately prior API call or NULL if no such information exists.

The following additional functions now set thead error detail on failure.

| Function                                 |
|------------------------------------------|
| ddca_get_non_table_vcp_value()           | 
| ddca_get_table_vcp_value()               | 
| ddca_get_any_value_using_explicit_type() |
| ddca_get_any_value_using_implicit_type() |
| ddca_get_capabilities_string()           |

<br>

### Miscellaneous

Move definitions of **DDCA_Build_Option_Flags** and **DDCA_Capture_Option_Flags** from **ddcutil_c_api.h** to **ddcutil_types.h**, to facilitate creation of a C++ API, as requested by J&uuml;rgen Altfeld. 

Some API functions returned ***DDCRC_ARG*** when passed an invalid parameter, others returned ***-EINVAL***.
All now return ***DDCRC_ARG***.

| Functions Affected                    |   
|---------------------------------------|
| ddca_set_max_tries()                  |
| ddca_create_mfg_model_sn_display_identifier() |
| ddca_create_edid_display_identifier() |
| ddca_create_display_ref()             |
| ddca_get_feature_list_by_dref()       |
| ddca_get_feature_name_by_dref()       |

<br>

API functions can no longer return ***DDCRC_UNINITIALIZED*** as initializion is guaranteed when the shared library is loaded

The following functions now return "const char \*" instead of "char \*" to emphasize that they return pointers into internal 
**ddcutil** data structures that should not be freed.  Note that this is a non-upwardly compatible API change that may 
require client changes.

|Function    |
|----------------------------|
|ddca_rc_name()              |
|ddca_rc_desc()              |
|ddca_did_repr()             |
|ddca_dref_repr()            |
|ddca_dh_repr()              |
|ddca_get_feature_name()     |
|ddca_feature_list_string()  |

<br>

### Precondition Failure

API function precondition failures reflect an error in the calling client program.
How best to deal with them?  Multiple design principles apply here.  Unfortunately,
they conflict:   
- Libraries should not crash  
- Programming errors should cause immediate and spectacular failure  
- Client programs should not have to test for return codes that
  reflect programming errors. This clutters the code.

Java addresses this conundrum through the use of RuntimeExceptions, such as NullPointerException
and IllegalArgumentException, which need not be caught, or can be caught at the top level of a
program.  Unfortunately, C doesn't provide such an elegant solution. 

Until now, many precondition tests have been done using assert() statements.
This is not a long term solution, since assert()s can be disabled when programs are built for production. 
Other precondition failures have resulted in a return code of ***DDCRC_ARG***.

As a first step, the assert() statements and other precondition tests have been unified
with a common set of macros.  This at least provides more consistent behavior, and will facilitate future changes.
For now, a precondition
failure results in a message to stderr and a return code of ***DDCRC_ARG***.  In the future,
this may change to a message followed by library abort.  Or the behavior may be controlled by 
the client using an API call to select the desired behavior.  Comments on this design issue are invited.


### Notes on Freeing Resources

**ddca_free_** functions fall into two groups, those that specify client side data structures and are referenced by a normal pointer, and those that specify internal library data structures and are referenced by an opaque pointer. 

Those that specify client side data structures are essentially convenience functions, relieving the client programmer of the details of freeing what may be a complex data structure.  These functions always return void.  If passed a NULL pointer, they do nothing.  

|Functions that free client data structures | 
|-------------------------------------------|
| ddca_free_error_detail()        | 
| ddca_free_display_info_list()   | 
| ddca_free_parsed_capabilities() | 
| ddca_free_table_vcp_value()     | 
| ddca_free_any_vcp_value()       |  
| ddca_free_feature_metadata()    |

Functions that free internal library data structures take an opaque pointer as an argument.  These functions return a **DDCA_Status**.  They return 
***DDCRC_ARG*** if the pointer is invalid.  If the pointer is NULL, the function does nothing and ***DDCRC_OK*** is returned. Depending on the function, additional status codes are possible.  Whether or not memory is actually freed is a hidden implementation detail, as some opaque pointers reference persistent internal data structures.    

|Functions that free internal library data structures |
|------------------------------------------------------|
|ddca_free_display_identifier() | 
|ddca_close_display()           |


Additional Notes: 

If a function that creates a resource (e.g. **ddca_create_busno_display_identifier()**) returns other than ***DDCRC_OK*** (0), then the resource is not created and need not be freed.

In general, the order in which the ***ddca_free_*** functions are called does not matter.  

<!--
The one exception is that a **DDCA_Display_Ref** should not be freed if it is associated with an open **DDCA_Display_Handle**.   A call to **ddca_free_display_ref()** when the display reference is referenced by an open display handle returns ***DDCRC_LOCKED***. 
-->

The **ddca_free_** functions are not idempotent.  Attempting to free an already freed opaque resource will likely return ***DDCA_ARG***, but may result in a segfault.  Attempting to free an already freed client side resource will also likely result in a segfault.

### Newly Deprecated Functions

API function **ddca_get_formatted_vcp_value()** is marked as deprecated and will be removed in a future release.  It does not support user defined data types.


### Previously Deprecated Functions Deleted

The following functions and type definitions, along with related constants, were deprecated in Release 0.9.0.
They have now been deleted from files **ddcutil_c_api.h** and **ddcutil_types.h***.  

|Function or Typedef Deleted | Comment |
|----------------------------|------------------|
|DDCA_MCCS_Version_Id            |&nbsp; use DDCA_MCCS_Version_Spec |
|ddca_get_mccs_version_id_name() | |
|ddca_get_mccs_version_id_desc() |  |
|ddca_get_display_info_list()    |&nbsp; use ddca_get_display_info_list2() |
|ddca_get_simple_feature_value_name_by_display() |&nbsp; obtain from DDCA_Feature_Metadata|
|ddca_get_edid_by_dref()         |&nbsp; obtain from DDCA_Feature_Metadata|
|ddca_open_display()             |&nbsp; use ddca_open_display2()           |
|ddca_set_simple_nc_vcp_value()  |&nbsp; use ddca_set_non_table_vcp_value() |
|ddca_set_continuous_vcp_value() |&nbsp; use ddca_set_non_table_vcp_value() |

<br>

### Sample Programs

The sample client programs have been modified to reflect the API changes.

Fixed an incorrect printf() format string in sample program demo_get_set_vcp.c.  Reported by J&uuml;rgen Altfeld.
