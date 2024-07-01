## Feature Selection

Commands **getvcp**, **setvcp**, and **vcpinfo** operate on a single VCP feature or (as of release 1.3.0) on multiple VCP features.
Additionally, **getvcp** and **vcpinfo** can specify a named group of features. 

The ***feature-code*** argument to **vcpinfo**, **getvcp**, and **setvcp** is a hexadecimal feature number, 
with or without a leading "x" or "0x", for example:
~~~
$ ddcutil getvcp 10
$ ddcutil vcpinfo x10
$ ddcutil getvcp 0x10

~~~
 
The ***feature-group*** argument to **getvcp** and **vcpinfo** is a named collection of features.  
The most useful are: 

| Name     | Description |
|----------|-------------|
| ***KNOWN***    | &nbsp;feature codes defined in MCCS |
| ***SCAN***     | &nbsp;scan all feature codes x00..xff |
| ***COLOR***    | &nbsp;color related features   |
| ***PROFILE***  | &nbsp;features saved by dumpvcp (subset of COLOR) |
| ***TABLE***    | &nbsp;features of type Table |

<br>
For example:
~~~
$ ddcutil getvcp known
~~~

There are many more feature groups.  Most were created to help analyze the Monitor Control Command Set specification, and are not of general interest.
For a complete list of feature groups, use option **--help --verbose**. 

The following options subset a ***feature-group***: 

### Option: --no-table<a name="no-table"></a>

Exclude table type feature codes.  This applies when a feature group is specfied,
unless it is the ***TABLE*** feature group.  It does not apply when a single feature code is specified.
This is the default.

### Option: --show-table<a name='show-table'></a>

Include table type feature codes in all contexts.

### Option: ***--show-table***

Features of type ***Table*** exist only in Monitor Control Command Spec Version 3.0, which no known monitor has implemented.
Performing **getvcp** on a feature of type ***Table*** which does not exist is costly in terms of elapsed time.  Therefore, 
***Table*** type features are excluded from all feature groups except for group ***SCAN***, which tries to read every possible
feature code except for those known to be write-only, and group ***TABLE***, as that would be nonsensical.

This option causes such features to be included in all feature groups.

### Option: --rw <a name='rw'></a>

Include only feature codes that are read-write.

### Option: --ro <a name='ro'></a>

Include only feature codes that are read-only. 

### Option: --wo<a name='wo'></a>

Include only feature codes that are write-only. Applies only to **vcpinfo**.