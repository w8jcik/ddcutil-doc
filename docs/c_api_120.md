## Shared Library Changes for Release 1.2.0/1.2.1/1.2.2

### Overview

This release contains a handful of changes, which affect backward compatibility. 
Consequently, the ***libddcutil*** SONAME is now libddcutil.so.4. 
The shared library name is **libddcutil.4.3.0**.
In conformance with Debian practice, the package name is ***libddcutil4***.

#### **libddcutil** trace file

**libddcutil** can redirect all output that it would normally be sent to the terminal to a trace file instead. 
The file is specified using option ***--libddcutil-trace-file***.  This option can be only
be specified in the ***[libddcutil]*** section of the **ddcutil** configuration file, **ddcutilrc**.
The file name given can be either absolute or relative. If relative, the directory is resolved as per the
definition of XDG_STATE_HOME in the XDG specification.  Typically this is $HOME/.local/state/ddcutil.
This allows for tracing in 
situations where there is no terminal to which to write, e.g. PowerDevil in KDE Plasma.

### API changes


Structs and Enums             | Comments
------------------------------|----------------
enum DDCA_OUTPUT_LEVEL        |&nbsp;add value DDCA_OL_VV
type DDCA_TRACE_OPTIONS       |&nbsp;add value DDCA_TRCOPT_WALLTIME (release 1.2.1)

<br>

New Function                     | Comments
---------------------------------|----------
ddca_add_trace_groups            |&nbsp;Add to the set of traced groups
ddca_get_extended_version_string |&nbsp;Version string with optional suffix, e.g. "1.1.2-rc2"
ddca_redetect_displays           |&nbsp;Redetect displays
ddca_get_display_refs            |&nbsp;Returns a null-terminated list of valid DDCA_Display_Refs
ddca_get_display_info            |&nbsp;Returns DDCA_Display_Info for a DDCA_Display_Ref

<br>
Notes: 

- The existing API function ***ddca_set_trace_groups()*** replaces the existing set of trace groups,
whereas new API function ***ddca_add_trace_groups()*** adds to the current set.
- Calling ***ddca_get_display_refs()*** followed by calling
***ddca_get_display_info()*** for each display is preferred over ***ddca_get_display_info_list2()***.
Experience has shown that it leads to cleaner client code.

<br>

Changed Function               |  Comments 
-------------------------------|----------------------
ddca_report_display_info()     |

<br>

New Macro       | Comments          |
----------------|-------------------|
DDCUTIL_VSUFFIX |&nbsp;version suffix, may be "" 
 
<br>

### Miscellaneous **libddcutil** changes

- Additional checks are performed on **DDCA_Display_Ref** and **DDCA_Display_Handle** function arguments.  
- ***assert{}*** statements are used in **libddcutil** for situations that 
should be logically impossible.  To aid in remote problem diagnosis, many of these statements have been 
replaced by macro TRACE_ASSERT(), which additionally writes a messsage
to both the **libddcutil** trace log and the system log.
- Recognize configuration file option ***--libddcutil-trace-file***
***--library-trace-file***

