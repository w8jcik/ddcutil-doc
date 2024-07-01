## Shared Library Changes for Release 0.9.9

### Overview

Ths release contains a few non-upwardly changes. 
Consequently, the **libddcutil** SONAME is now libddcutil.so.3. 
In conformance with Debian practice, the package name is ***libddcutil3***.


### API Changes

#### General API Changes

| Structs and Enums       | Change                               |
|------------------------|----------------------------------------------
| enum DDCA_Trace_Group        | add value DDCA_TRC_RETRY (small subset of DDC_TRC_IO)
| enum DDCA_Trace_Options      | change DDCA_TRCOPT_TIMESTAMP value from x00 to x01 so enum values can be or'd
| enum DDCA_Feature_Subset_Id  | add DDCA_SUBSET_CUSTOM
| struct DDCA_Capabilities     | add fields: messages, msg_ct



| Modified Functions                |  Comments 
|-----------------------------------|------------------
| ddca_parse_capbabilities_string() | use new DDCA_Capabilities fields ***messages***, ***msg_ct*** to return messages instead of writing them to the terminal
| ddca_parse_capbilities_string()   | return status code DDCRC_BAD_DATA instead of DDCRC_OTHER

<br>

#### **DDCA_Feature_List** Functions

Most **DDCA_Feature_List** functions now take
***DDCA_Feature_List*** arguments instead of pointers to ***DDCA_Feature_List***.
Always using pointers had made the parameter lists more consistent, but didn't improve clarity. 

|New DDCA_Feature_List Functions                 | Comments
|------------------------------|------------------
| ddca_feature_list_eq()              | compare 2 feature lists for equality

<br>

|Modified DDCA_Feature_List Functions            | Comments
|-----------------------   ----|------------------
|ddca_feature_list_add()       | returns modified feature list instead of void
|ddca_feature_list_containts() | use DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature_list_eq()        | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature_list_or()        | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature_list_and()       | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature-list_and_not()   | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature_list_count       | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 
|ddca_feature_list_string      | has DDCA_Feature_List as parm type instead of *DDCA_Feature_List 


#### Tuning

The sleep multiplication factor is now set on a per-thread basis

| New Functions                 | Comments
|-----------------------   ----|------------------
| ddca_get_default_sleep_multiplier() | set sleep multiplication for new threads
| ddca_set_default_sleep_multiplier() | get sleep multiplication factor for new threads
| ddca_get_sleep_multiplier()         | set sleep multiplication factor for current thread
| ddca_set_sleep_multiplier()         | get sleep multiplication factor for current thread
| ddca_enable_sleep_suppression()     | corresponds to option --sleep-less
| ddca_is_sleep_suppression_enabled() | 

<br>

| Deprecated Functions                   | Use Instead
|-----------------------         ----|--------|
| ddca_set_global_sleep_multiplier() | ddca_set_default_sleep_multiplier() | 
| ddca_get_global_sleep_multiplier() | ddca_get_default_sleep_multiplier() | 

<br>

#### Statistics 

Statistics are now gathered on a per-thread basis.

| Modified Functions                       | Comments
|-------------------------------------|--------------------------|
| ddca_show_stats()                 | add parm ***include_per_thread_data***


|New Functions                   |  Comment
|--------------------------------|------------------------|
|ddca_set_thread_description()   | for use in statistics reports
|ddca_append_thread_description  | append to existing thread description
|ddca_get_thread_description     | retrieve thread description

<br>

#### **libddcutil** Development Package

In addition to changes to files **ddca_types.h** and **ddca_c_api.h**, 
the **libddcutil** development package, named **libddcutil-dev** on some platforms and **libddcutil-devel** on others, 
has the following changes.

- The pkg-config file **ddcutil.pc** is not architecture agnostic. 
Therefore, it is not installed in the $(datadir) directory (typically /usr/share/pkgconfig).
Instead it is installed in the architecture specific ${libdir} directory, e.g. /usr/lib64/x86_64-linux-gnu in Debian/Ubuntu

- Cmake configuration file ***FindDDCUtil.cmake*** is now installed in /usr/share/cmake/Modules, not the **ddcutil** data directory 
(typically /usr/share/ddcutil/data), where it was left for the user to install in the proper location.

