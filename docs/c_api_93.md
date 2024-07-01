## C API Changes for Release 0.9.3

### Trace Control

API functions to control tracing have been added.  This API corresponds to
ddcutil command line options ***--trace***, ***--trcfunc***, and ***--trcfile.***

Added enum DDCA_Trace_Group, whose values can be ORed together, to specify 
the classes to be traced.

| Flag           | Meaning                                 |
|----------------|-----------------------------------------|
| DDCA_TRC_BASE  | Base functions                          |
| DDCA_TRC_I2C   | I2C layer                               |
| DDCA_TRC_ADL   | ADL layer                               |
| DDCA_TRC_DDC   | Higher level functions in the DDC layer |
| DDCA_TRC_USB   | USB connecte didplay                    |
| DDCA_TRC_TOP   | **ddcutil** mailine **                  |
| DDCA_TRC_ENV   | ***environment*** command               |
| DDCA_TRC_API   | top level API functions                 |
| DDCA_TRC_UDF   | user-defined functions                  |
| DDCA_TRC_VCP   | VCP layer, feature definitions          |
| DDCA_TRC_DDCIO | Low level functions in the DDC layer    |
| DDCA_TRC_NONE  | all tracing disabled                    |
| DDCA_TRC_ALL   | all tracing enabled                     |


| New Function               | Comments
|----------------------------|----------------|
| ddca_add_traced_function() | turn on tracing for a specific function |
| ddca_add_traced_file()     | turn on tracing for a specific source file |
| ddca_set_trace_groups()    | turn on tracing for one or more trace groups |


### Feature Metadata

To allow for user supplied feature definitions, metadata requests are now generally tied to the display, by
specifying a ***DDCA_Display_Ref*** or ***DDCA_Display_Handle*** instead of the VCP version.  That is, when metadata
is requested for a feature, we first look to see if there is a user supplied feature definition for the display. Absent that,
the VCP version of the display is used to retrieve the appropriate feature definition. 

Eliminate typedef ***DDCA_Feature_Value_Table***, which is defined as ***DDCA_Feature_Value_Entry*** *, and is used to reference a feature value lookup table.  Use ***DDCA_Feature_Value_Entry*** * instead.  

Functions whose signatures are affected: 

| Function                                          | Comments                                                    |
|--------------------------------------------------|-------------------------------------------------------------|
| ddca_get_simple_nc_feature_value_name_by_table() |  |



| Deleted Structs and Functions                      | Comments                                                    |
|----------------------------------------------------|-------------------------------------------------------------|
| struct DDCA_Version_Feature_Info                   | used by feature_info functions
| ddca_get_feature_info_by_vcp_version()             | deprecated in 0.9.0, returned DDCA_Version_Feature_Info |
| ddca_get_feature_info_by_display()                 | deprecated in 0.9.0, returned DDCA_Version_Feature_Info |
| ddca_free_feature_info()                           | deprecated in 0.9.0 | 
| ddca_get_mccs_version_id()                         | deprecated in 0.9.0 |
| ddca_get_simple_sl_value_table()                   | deprecated in 0.9.0, use ddca_get_feature_metadata...() variant  |
| ddca_get_simple_nc_feature_value_name_by_display() | deprecated in 0.9.0 | 



| Deprecated                             | Comments                          |
|----------------------------------------|-----------------------------------|
| ddca_get_feature_name_by_dref()        | use ddca_get_feature_metadata()   | 
| ddca_free_metadata_contents()          | use ddca_free_feature_metadata()  |


| Renamed Function                   | Comments |
|------------------------------------|------------|
| ddca_get_feature_name_by_dref()    | was ddca_feature_name_by_dref() |



| Modified Functions                   | Comments |
|--------------------------------------|------------|
| ddca_get_feature_metadata_by_vspec() | returns pointer to new ***DDCA_Feature_Metadata*** |
| ddca_get_feature_metadata_by_dref()  | returns pointer to new ***DDCA_Feature_Metadata*** |
| ddca_get_feature_metadata_by_dh()    | returns pointer to new ***DDCA_Feature_Metadata*** |

The above functions now take a ***DDCA_Metadata\*\**** instead of a ***DDCA_Metadata\**** parm, 
i.e. they allocate a new DDCA_Metadata instance instance instead of filling in an 
exisitng instance.


| Funcations Added                  | Comments |
|--------|------------|
| ddca_free_feature_metadata()              | replaces ddca_free_metadata_contents() | 
| ddca_enable_udf()                         | controls whether user supplied feature definitions are enabled |
| ddca_is_udf_enabled()                     | reports whether usr supplied feature definitions are enabled |
| ddca_report_capabilities_by_dref()        | handles display-specific user supplied feature definitions |
| ddca_report_capabilities_by_dh()          | handles display-specific user supplied feature definitions |
| ddca_check_by_dref()                      | ensures user supplied feature defintions are loaded  |
| ddca_check_by_dh()                        | ensures user supplied feature defintions are loaded |
| ddca_free_feature_metadata()              | |
| ddca_report_parsed_capabilities_by_dref() | uses user supplied feature definitions |
| ddca_report_parsed_capabilities_by_dh()   | uses user supplied feature definitions  | 


The following flags have been added to field **feature_flags** in struct **DDCA_Feature_Metadata** 
or therir meaning may have been modified, and they may be visible when an instance is returned by the API: 

| Bit flag                               |                                                                 |
|----------------------------------------|-----------------------------------------------------------------|
| DDCA_SYNTHETIC                         | metadata was dynamically created for undefined feature          | 
| DDCA_USER_DEFINED                      | added, metadata is from user supplied feature definition        |


Sample files reflect signature changes of ddca_get_feature_metadata_by_vspec() etc


| File name              | Comments           |
|------------------------|--------------------|
| demo_capabilities.c    |
| demo_get_set_vcp.c     |
| demo_vcpinfo.c         |

### Miscellaneous 

| Functions Added            | Comments           |
|----------------------------|--------------------|
| ddca_report_error_detail() | Convenience function that formats **DDCA_Error_Detail** |  

Removed structs and typedefs related to experimental async support

| Removed                                       | Comments           |
|-----------------------------------------------|--------------------|
| enum DDCA_Queue_Request_Type                  | |
| struct DDCA_Queued_Request                    | |
| function prototype DDCA_Notification_Func     | |
| function prototype Simple_Callback_Func       | |





