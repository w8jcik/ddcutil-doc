## C API Changes for Release 0.9.0

See also:   
- [Main C API Page](api_main.md)  

The C API has been extensively revised reflecting experience gained from work on a Qt C++ GUI interface.

New header files:   

| File Name             |  Contents
|-----------------------|-------------------|
|ddcutil_macros.h       |&nbsp; Macros to specify the **ddcutil** version at compile time   
|ddcutil_status_codes.h |&nbsp; Status codes defined by **ddcutil**

<br>

#### Multithreading

There is rudiementary support for multi-threaded use of the API.

Considerations:

- It is safe to perform simultaneous operations on separate displays (using distinct display handles) on separate threads.   
- Multiple simultaneous DDC operations on the same display will have unpredictable results.  
- Do not open multiple handles for the same display.   
The **ddcutil** API prevents opening multiple handles for devices with the same DDCA_IO_Path, 
which blocks the most common case.  However, it is conceivable that the same monitor might be 
connected in multiple ways (e.g. with both a DDC and USB connection), with multiple cards, 
or even from separate computers.   In this case it is the application's or even user's responsibility
to ensure that conflict does not occur.   
- The following settings are now thread specific: 
    - whether **setvcp** verification is enabled (**ddca_enable_verify()**, **ddca_is_verify_enabled()**)
    - output redirection (**ddca_set_fout()**, etc.)
    - output level (**ddca_set_output_level()**, **ddca_get_output_level()**)


Presently, multi-threaded operation is best managed in the application. 
The C++ Qt based GUI currently under development does the following:   
- Calls **ddca_get_display_info_list()** to enumerate the displays  
    - Opens the display (i.e. creates a **DDCA_Display_Handle**)  
    - Maintains a queue of requests   
    - Calls the ddcutil API for each request, and updates the GUI accordingly 

#### Library Build Information

No changes.

|Unchanged                     | Comments           |
|------------------------------|--------------------|
|ddca_ddcutil_version()        |                    |
|ddca_ddcutil_version_string() |                    |
|ddca_build_options()          |                    |

<br>

#### Status Code Functions

No changes.

|Unchanged           |Comments            |
|--------------------|--------------------|
|ddca_rc_name()      |                    |
|ddca_rc_desc()      |                    |

<br>

#### Detailed Error Reports

In some cases, detailed error information is appopriate, particularly when there 
are multiple causes, each of which should be reported.  The following functions 
get and release detailed error reports for the most recent API call. 

Currently implemented only for **ddca_set_profile_related_values()**, reports 
each case of invalid data in the input and the line at which it occurred.


|New                 |Comments            |
|--------------------|--------------------|
|ddca_get_error_detail() |&nbsp; Get detailed error information for most recent API call |
|ddca_free_error_detai() |&nbsp; Free detailed error information | 

<br>

#### Global Settings 


|Unchanged            |  Comments       |
|---------------------|--------------------|
|ddca_max_max_tries() |                   |
|ddca_get_max_tries() |                   |
|ddca_set_max_tries() |                   |

|Changed                  | Comments
|-------------------------|--------------------|
|ddca_enable_verify()     |&nbsp; now thread-specific setting, returns prior value |
|ddca_is_verify_enabled() |&nbsp; now thread-specific setting |

<br>

#### Output Redirection and Capture

Output redirection is now thread-specific.

|Changed Functions          |  Comments           |
|---------------------------|---------------------|
|ddca_set_fout()            |&nbsp; now thread-specific |
|ddca_set_fout_to_default() |&nbsp; now thread-specific |
|ddca_set_ferr()            |&nbsp; now thread-specific |
|ddca_set_ferr_to_default() |&nbsp; now thread-specific |

<br>

Added convenience functions to capture output normally directed to the terminal. 
Output capture is thread specific.

| New                     | Comments                                       |
|-------------------------|------------------------------------------------|
|ddca_start_capture()     |&nbsp; Begin capturing output                         |
|ddca_end_capture()       |&nbsp; Terminates capture, returns the captured value |

<br>

For examples of output redirection, see sample program **demo_redirection.c**. 


#### Message Control

Output level settings are now thread-specific.

|Changed                          | Comments           |
|---------------------------------|--------------------|
|ddca_get_output_level()          |&nbsp; now thread-specific |
|ddca_set_output_level()          |&nbsp; now thread-specific, returns old level |

<br>

|Unchanged                           |Comments                          |
|------------------------------------|----------------------------------|
|ddca_output_level_name()            |                                  |

<br>

### Statistics and Diagnostics 

Statistics and diagnostics control are global, not thread-specific.

|Unchanged           | Comments           |
|--------------------|--------------------|
|ddca_reset_stats()  | | 
|ddca_show_stats()   | |
|ddca_is_report_ddc_errors_enabled() |  |

<br>

|Changed           | Comments           |
|--------------------|--------------------|
|ddca_enable_report_ddc_errors()  |&nbsp; returns old level                |

<br>

|New                          | Comments |
|-----------------------------|--------------------|
|ddca_enable_error_info()     |&nbsp; control display of internal exception reports |

<br>

#### Feature Lists

Struct ***DDCA_Feature_List*** specifies a set of VCP feature codes using a 256 bit array.

Functions and constants for operating on feature lists:

| New                           | Comments  |
|-------------------------------|--------------------------------|
|ddca_feature_list_add()        |&nbsp;add a VCP feature code to the list  |
|ddca_feature_list_and()        |&nbsp;returns intersection of 2 lists 
|ddca_feature_list_and_not()    |&nbsp;returns the features in one list that are not in a second|
|ddca_feature_list_clear()      |&nbsp;clear all entries in the feature list |
|ddca_feature_list_contains()   |&nbsp;check if a VCP feature code is in the list  |
|ddca_feature_list_count()      |&nbsp;number of features in list                |
|ddca_feature_list_id_name()    |&nbsp;get symbolic name of feature list id |
|ddca_feature_list_or()         |&nbsp;returns union of 2 feature lists |
|ddca_feature_list_string()     |&nbsp;returns a string representation of a feature list |
|ddca_get_feature_list_by_dref()|&nbsp;returns a feature list identifying the features in named feature set (e.g. COLOR) |
|DDCA_EMPTY_FEATURE_LIST        |&nbsp;an empty feature list |

<br>

For an example of using feature lists, see sample program **demo_feature_list.c**.

#### MCCS Version Identification

To date, the API has had two different ways to specify the Monitor Control Command Set (MCCS) version:  
- **DDCA_MCCS_Version_Spec**,  a pair of 8 bit unsigned integers.  
- **DDCA_MCCS_Version_Id**, an enum of valid MCCS versions.

Going forward, the API will use **DDCA_MCCS_Version_Spec**.  Existing functions using
**DDCA_MCCS_Version_Id** are retained, but are marked as deprecated and will be
removed in a future release.  Once the deprecated functions are removed, there
will likely be some simplification of the remaining function names.

Symbolic constants **DDCA_VSPEC_V10** etc. have been added for **DDCA_MCCS_Version_Spec** values. 

|Deprecated                   |Comments |
|-----------------------------|---------------------------------| 
|DDCA_MCCS_Version_Id         |&nbsp;MCCS version enum                      |
|ddca_mccs_version_id_name()  |                                        |
|ddca_mccs_version_id_desc()  |                                        |
|ddca_get_mccs_version_id()   |&nbsp;Get MCCS version id for display handle |

<br>

|Unchanged                    |Comments |
|-----------------------------|---------|
|ddca_get_mccs_version()      |&nbsp;takes DDCA_Display_Handle argument  |

<br>

Background:

Each way to specify the MCCS version has its advantages.  **DDCA_MCCS_Version_Spec** was chosen because
it is the form already used in the MCCS spec.

**DDCA_MCCS_Version_Spec**  
- Feature code xDF (get MCCS version) returns a number pair  
- The capabilities string specifies the MCCS version as a number pair

**DDCA_MCCS_Version_Id**  
- There are only a handful of valid version numbers  
- Simplifies version specification  
- Simplifies version comparisons, which are complicated by the fact  
that both Version 3.0 and Version 2.2 are independent successors to Version 2.1. 


#### Detect Displays

- **ddca_display_info_list2()** replaces **ddca_display_info_list()**, allows for including displays that don't support DDC, 
  signature returns a status code, conforming to standard API pattern
- **ddca_report_displays()** replaces **ddca_report_active_displays**, added parm to allow for reporting invalid displays
- Struct **DDCA_Display_Info** now has a field for the MCCS version.  


|Deprecated                      |Comments                |
|--------------------------------|------------------------|
|ddca_get_display_info_list()    |&nbsp;Use ddca_get_display_info_list2() |
|ddca_report_active_displays()   |&nbsp;Use ddca_report_displays()        |

<br>

|Unchanged                     | Comments | 
|------------------------------|----------------|
|ddca_report_display_info()      |       |
|ddca_report_display_info_list() |  |
|ddca_free_display_info_list() | |

<br>

|New                             |Comments |
|--------------------------------|----------|
|ddca_get_display_info_list2()   |&nbsp; Can include displays that don't support DDC |
|ddca_report_display_by_dref()   |&nbsp; Report display information using code in **ddcutil detect** |
|ddca_report_displays()          |&nbsp; Optionally include displays that don't support DDC |

<br>

#### Display Identifiers 

No changes. 

|Unchanged Functions                           | Comments | 
|----------------------------------------------|----------------|
|ddca_create_dispno_display_identifier()       | | 
|ddca_create_busno_display_identifier()        | |
|ddca_create_adlno_display_identifier()        | | 
|ddca_create_mfg_model_sn_display_identifier() | |
|ddca_create_edid_display_identifier()         | | 
|ddca_create_usb_display_identifier()          | |
|ddca_create_usb_hiddev_display_identifier()   | | 
|ddca_free_display_identifier()                | |
|ddca_did_repr()                               | | 

<br>

#### Display References 

|Deprecated                      |Comments                |
|--------------------------------|------------------------|
|ddca_get_edid_by_dref()         |&nbsp;renamed, was ddca_get_edid_by_display_reference() |

<br>

|Unchanged                 | Comments | 
|--------------------------|----------------|
|ddca_create_display_ref() | | 
|ddca_free_display_ref()   | |
|ddca_dref_repr()          | |

|Changed                        | Comments | 
|-------------------------------|----------------|
|ddca_dbgrpt_display_ref()      |&nbsp;renamed, was ddca_report_display_ref()            |

<br>

|New                           | Comments |
|------------------------------|----------------| 
|ddca_report_display_by_dref() | | 

<br>

#### Display Handles

|Deprecated API Functions         |Comments                |
|---------------------------------|------------------------|
|ddca_open_display()              |&nbsp;Use ddca_open_display2() |

|Unchanged                        | Comments | 
|---------------------------------|----------------|
|ddca_close_display()             | | 
|ddca_dh_repr()                   |           | 

|New                              |Comments |
|---------------------------------|-------------------------------|
|ddca_open_display2()             |&nbsp; allows for waiting if display locked by another thread | 
|ddca_display_ref_from_handle()   | | 

<br>

#### Capabilities String

|Unchanged                          | Comments | 
|-----------------------------------|----------------|
|ddca_get_capabilities_string()     |   |
|ddca_parse_capabilities_string()   | |
|ddca_free_parsed_capabilities()    | |

<br>

|Changed                            | Comments                          |
|-----------------------------------|-----------------------------------|
|ddca_report_parsed_capabilities()  |&nbsp; interprets feature and value codes|

<br>

|Added                              |Comments |
|-----------------------------------|-----------------------------------|
|ddca_feature_list_from_capabilities() |&nbsp; Create feature id bitifield from capabilities |

<br>

#### Feature Metadata

Struct **DDCA_Version_Feature_Info** and related functions are deprecated.  Instead, the master 
record for feature metadata is now **DDCA_Feature_Metadata**.  


|Deprecated and Deleted                  |Comments |
|----------------------------------------|-----------------------------------------------------------------------|
|DDCA_Version_Feature_Info               |&nbsp; use DDCA_Feature_Metadata |
|ddca_get_feature_info_by_vcp_version()  |&nbsp; Use ddca_get_feature_metadata()  - in 0.8.7
|ddca_get_feature_info_by_display()      |&nbsp; Use ddca_get_feature_metadata()   |
|ddca_free_feature_info()                |&nbsp; in 0.8.7? 
|ddca_get_simple_sl_value_table()        |&nbsp; Takes a DDCA_MCCS_Version_Id argument. |

<br>

|Unchanged                               |Comments |
|----------------------------------------|----------|
|ddca_get_feature_name()                 |          |

<br>

|Added Structs and Functions                      |Comments                            |
|-------------------------------------------------|------------------------------------|
|DDCA_Feature_Metadata                            |&nbsp;replaces DDCA_Version_Feature_Info  |
|ddca_get_feature_metadata_by_dref()              |                                    |
|ddca_get_feature_metadata_by_dh()                |                                    | 
|ddca_free_feature_metadata_contents()            |                                    | 
 
<br>

|Additional new granular functions              | Comments                           |
|--------------------------------------------------|-----------------------------------|
|ddca_feature_name_by_dref()               |                                    | 
|ddca_get_simple_sl_value_table_by_dref()  |&nbsp; Gets table of valid values if flag **DDCA_SIMPLE_NC** is set in **DDCA_Feature_Flags**.   |

<br>

Miscellaneous changes:  
- Bitfield **DDCA_Version_Feature_Flags** now has bit **DDCA_NC_COMPLEX**, indicating a VCP code
(designated by MCCS as Non-Continuous) that uses certain values as an NC feature and uses the 
remaining range for continyous adjustment. An example of such a feature is VCP code x62 (Audio Speaker Volume)
as defined in MCCS versions 3.0 and 2.2, where value x00 is defined as "Fixed (default level)", value xFF is "Mute", 
and values x01..xFE provide a continuous range for volume adjustment. 



#### NC Feature Value Lookup 

(See Metadata section for discussion of getting lookup tables)

|Deprecated and Deleted Functions                   |Comments |
|---------------------------------------------------|-----------------|
|ddca_get_simple_nc_feature_value_name()            |&nbsp; Get feature metadata, then use ddca_get_simple_nc_feature_name_by_table() |
|ddca_get_simple_nc_feature_value_name_by_display() |&nbsp; Get feature metadata, then use ddca_get_simple_nc_feature_name_by_table() |

<br>

|New                                                | Comments | 
|---------------------------------------------------|-----------|
|ddca_get_simple_nc_feature_value_name_by_table()   |&nbsp; new |

<br>



#### Symbol Name Changes 

Numerous symbols have been renamed for clarity and consistency.
General changes:  
- The names of report functions that display internal data structures for debugging, as opposed to reports
designed to be shown to users, now begin with "ddca_dbgrpt_" instead of "ddca_report_".   
- Enum values **DDCA_V10** etc. are now **DDCA_MCCS_V10** etc. (but note that the use of **DDCA_MCCS_Version_Id** is deprecated 
in favor of **DDCA_MCCS_Version_Spec**)  
- Names of parameters that specify the address at which a pointer is to be returned now generally end in "_loc".

Specific changes not listed elsewhere: 

|Old Name                          |New Name        |
|----------------------------------|----------------|
|DDCA_IO_DEVI2C                    |&nbsp; DDCA_IO_I2C                     |

<br>

Naming patterns:   
- Functions that always return the same value generally do not include "***\_get\_***" in their names,
e.g. **ddca_ddcutil_version()**, **ddca_max_max_tries()**   
- In the case of "overloaded" functions, "***\_by\_***" is usually used to distinguish variants,
e.g. **ddca_get_feature_metadata_by_vspec()**, **ddca_get_feature_metadata_by_dref()**, **get_feature_metadata_by_display()**  
- However, in the case where there the result depends on a single input value from which the result
is extracted, "***\_from\_***" is used,
e.g. **ddca_mmk_from_dref()**, **ddca_mmk_from_dh()** 




#### Get and Set VCP Feature Values 

**DDCA_Any_Vcp_Value** vs **DDCA_Non_Table_Vcp_Value**

The API provides functions for getting, setting, and otherwise processing VCP values in 3 forms, 
using the type specific **DDCA_Non_Table_Vcp_Value** or **DDCA_Table_Vcp_Value** or the more 
general **DDCA_Any_Vcp_Value**.   The first form is simpler to use, and does not entail the 
allocation of arbitrarily size buffers. 

As a practical matter, while the MCCS spec defines Table type features, they have not been observed
on any monitors.  Supporting Table type features in a UI adds complexity.
API users will probably want to use the simpler **DDCA_Non_Table_Vcp_Value** functions.

The following tables summarize the structs and API functions for getting and setting VCP feature values.

| Deprecated                                  | Comments 
|---------------------------------------------|---------------------|
|ddca_set_simple_nc_vcp_value()               |&nbsp; unchanged, but deprecated |
|ddca_set_continuous_vcp_value()              |&nbsp; unchanged, but deprecated |

<br>

| Changed                                     | Comments |
|---------------------------------------------|---------------------|
|DDCA_Non_Table_Vcp_Value                     |&nbsp; renamed, was DDCA_Non_Table_Value        |
|DDCA_Table_Vcp_Value                         |&nbsp; renamed, was DDCA_Table_Value |
|ddca_get_non_table_vcp_value()               |&nbsp; renamed, was ddca_get_nontable_vcp_value() |
|ddca_get_any_vcp_value_using_explicit_type() |&nbsp; renamed, was ddca_get_any_vcp_value(), parm type change |
|ddca_set_non_table_vcp_value()               |&nbsp; renamed, was ddca_set_raw_vcp_value() |
|ddca_get_table_vcp_value()                   |&nbsp; parameter change |
|ddca_free_table_vcp_value()                  |&nbsp; renamed from ddca_free_table_value_response() |

<br>

| New                                         | Comments  |
|---------------------------------------------|---------------------|
|ddca_get_any_vcp_value_using_implicit_type() |&nbsp; new function |
|ddca_set_table_vcp_value()                   |&nbsp; new |
|ddca_set_any_vcp_value()                     |&nbsp; new |
|ddca_free_any_vcp_value()                    |&nbsp; new |
|dbgrpt_any_vcp_value()                       |&nbsp; new |

<br>

#### Get or Set Multiple Values 

| Unchanged                                   | Comments            |
|---------------------------------------------|---------------------|
|ddca_get_profile_related_values() | |

<br>

|Changed                            |Comments                         |
|-----------------------------------|---------------------------------|
|ddca_set_profile_related_values()  |&nbsp; add display handle parm    |

<br>

if **ddca_set_profile_related_values()** returns **DDCRC_BAD_DATA**, **ddca_get_error_detail()** 
returns a detailed error report.


#### Format Feature Values

Functions **ddca_format_table_vcp_value()**, **ddca_format_non_table_vcp_value()**, and **ddca_format_any_vcp_value()** 
format a feature value, based on its feature code and MCCS version.  

Function **ddca_get_formatted_vcp_value()** continues to exist, but is essentially a convenience function
that combines reading a VCP value with formatting.

| Unchanged                                   | Comments            |
|---------------------------------------------|---------------------|
|ddca_get_formatted_vcp_value()               |&nbsp;unchanged |

<br>

|New                                          | Comments 
|---------------------------------------------|---------------------|
|ddca_format_non_table_vcp_value_by_dref()    |&nbsp; new |
|ddca_format_table_vcp_value_by_dref()        |&nbsp; new |
|ddca_format_any_vcp_value_by_dref()          |&nbsp; new |

<br>

#### Miscellaneous Changes 

- **ddca_enable_report_ddc_errors()**, **ddca_enable_verify()**, **ddca_set_output_level**
now return the prior setting  

#### Sample programs

Sample programs have been revised to reflect API changes and new sample programs added.  See the main [C API page](api_main.md#Documentation).

