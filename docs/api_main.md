## C API

For details of API changes, see [Shared Library Release Notes](libddcutil_release_notes.md)  
  
### Naming

Symbol names begin with "ddca_" or "DDCA_" (for **DDC**util **A**pi).

### Installation

Header files are located in the src/public directory.
They are normally installed to /usr/include or /usr/local/include. 

| &nbsp; Header File Name      | &nbsp; Contains |
|-----------------------|----------|
| &nbsp;ddcutil_types.h        | &nbsp;typedefs, structs, enums, etc. |
| &nbsp;ddcutil_c_api.h        | &nbsp;API functions
| &nbsp;ddcutil_status_codes.h | &nbsp;**ddcutil** specific status codes |
| &nbsp;ddcutil_macros.h       | &nbsp;macros to specify **ddcutil** version at compile time |

<br>
The API is exposed using shared library libddcutil.so. Note that the command line version of **ddcutil**
does not itself use the shared library, but instead links all services statically.

Installation (make install, package install) installs a Package Config file (ddcutil.pc). 

Installation also saves a cmake file (FindDDCUtil.cmake) in directory /usr/share/ddcutil/data 
(or /usr/local/share/ddcutil/data if appropriate).  To use this file, copy it to 
/usr/share/cmake/Modules.
 
### API Documentation

- The header files contain extensive documentation.   
- Doxygen documentation can be generated (if Doxygen is installed) using scripts in the doxydoc directory.
 However, you'll likely find  the comments in the header files and sample code easier to use.

Sample programs are found in directory src/sample_clients:

| &nbsp; Source file               | &nbsp; Contents                           |
----------------------------|------------------------------------|
| &nbsp;demo_capabilities.c        | &nbsp; Query monitor capabilities string  |
| &nbsp;demo_display_selection.c   | &nbsp; Select display                     |
| &nbsp;demo_feature_list.c        | &nbsp; Using DDCA_Feature_List            |
| &nbsp;demo_get_set_vcp.c         | &nbsp; Read and write VCP feature values  |
| &nbsp;demo_global_settings.c     | &nbsp; Query and change global settings   |
| &nbsp;demo_profile_features.c    | &nbsp; Save and restore color profile related features |
| &nbsp;demo_redirection.c         | &nbsp; Capture program output             |
| &nbsp;demo_vcpinfo.c             | &nbsp; Query VCP feature metadata         |

<br>
To build the sample client programs, issue the command ***make check***.

<!-- 
### Miscellaneous

Site [ABI Laboratory](https://abi-laboratory.pro/index.php?view=timeline&l=libddcutil) tests for binary compatility between shared library releases.
Note that there have been some 

-->
