## Table Features

Some VCP features are designated as type Table (T), as opposed to Continuous (C) or Non-Continuous (NC).
Read and write requests for table features transmit blocks of data.
In particular, LUT related features are of type Table. 

**ddcutil** exposes table feature values as uninterpreted hex strings,
e.g. 
~~~
ddcutil setvcp 60 01020304F1F2FEFF
~~~

Table features support is currently implemented over DDC/CI, but has never been tested with an actual monitor. 


To see the list of Table features, issue the command: 
~~~
ddcutil vcpinfo table
~~~

Note that whether a feature is of type Table can vary with MCCS version.  The `ddcutil vcpinfo` command tries to
take this into account.

| Code | Feature                           | Version Detail
|------:| ---------------------------------|-------------------------------------------------
|60    | Select Active Video Source        | Type NC in Versions 2.0 through 2.2 |
|a4    | Control Selected Window Operation | Type NC in MCCS 2.0, T in 3.0, 2.2 |
|b4    | Host time mode                    | Type NC in MCCS 2.0, 2.1, T in 3.0, 2.2
|d0    | Select Active Output              | Type T in MCCS 3.0, NC otherwise 

