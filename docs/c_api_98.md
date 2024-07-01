## Shared Library Changes for Release 0.9.8

#### SONAME

The **libddcutil** SONAME is now libddcutil.so.2.  It was incremented because 
libddcutil 0.9.8 is not backward compatible with release 0.9.7.
In conformance with Debian practice, the package name is now ***libddcutil2***. 

#### API Changes

| Structs and Enums            | Change                               |
|------------------------------|----------------------------------------------
| enum DDCA_Trace_Group        |&nbsp; added value DDCA_TRC_SLEEP to trace sleep operations
| enum DDCA_Trace_Options      |&nbsp; added DDCA_TRCOPT_TIMESTAMP: trace messages should show timestamps
| enum DDCA_Trace_Options      |&nbsp; added DDCA_TRCOPT_THREAD_ID  trace messages should show thread id
| struct DDCA_Feature_Metadata |&nbsp; added field vcp_version

<br>

Note: The change to struct **DDCA_Feature_Metadata** is not backward ABI compatible, resulting in the 
SONAME bump.

| New Functions                       | Comments
|-------------------------------------|--------------------------|
| ddca_get_global_sleep_multiplier()  |&nbsp; set sleep multiplication factor
| ddca_set_glolbal_sleep_multiplier() |&nbsp; get sleep multiplication factor
| ddca_set_trace_options()            |&nbsp; set trace message options
| ddca_dbgrpt_feature_metadata()      |&nbsp; convenience function for clients

<br>
#### **libddcutil** Development Package

The **libddcutil** development package, named **libddcutil-dev** on some platforms and **libddcutil-devel** on
others, has the following changes.

- Package config file ***ddcutil.pc*** is now installed in directory /usr/share/pkgconfig.  Previously it was 
installed in the **ddcutil** data directory, typically /usr/share/ddcutil/data, and left for the user to 
install in the proper location.    
- Cmake configuration file ***FindDDCUtil.cmake*** is now installed in /usr/share/cmake/Modules, not the **ddcutil** data directory 
(typically /usr/share/ddcutil/data), where it was left for the user to install in the proper location. 
