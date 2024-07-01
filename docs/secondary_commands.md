## Secondary Commands

Secondary commands explore the **ddcutil** environment for diagnostic purposes, or perform other specialized functions.

### Command: environment<a name="environment"></a>

Explores aspects of the system relevant to DDC communication (other than USB).  Normal output provides information that may be helpful to the user
in diagnosing things that need to be changed to enable ***ddcutil*** to execute.  Option ***--verbose*** adds additional information
that is helpful for remotely diagnosing **ddcutil** problems.

### Command: usbenvironment<a name="usbenvironment"></a>

Explores USB related aspects of the **ddcutil** execution environment.  See [USB Connected Monitors](usb.md).

### Command: probe<a name="probe"></a>

Gathers monitor specific infomration, including the EDID and the VCP feature codes supported by the monitor.  If there are multiple
monitors, the monitor to be probed is specified in the usual way (option ***--display***, etc.) 
Option ***--verbose*** increases the amount of information.

### Command: interrogate<a name="interrogate"></a>

The granddaddy of all diagnostic functions.  Includes vebose output from **detect**, **environment**, and for each monitor **probe**.
For remote problem diagnosis, **ddcutil environment --verbose** is usually sufficient. 

### Command: chkusbmon<a name="chkusbmon"></a>

This command is used by UDEV rules.  It checks if a USB device is a monitor.  See [Command ddcutil chkusbmon](usb.md#command_chkusbmon)

### Command: discard [capabilites|dsa|all] caches<a name="command_discard_caches"></a>

Discard cached capabilities strings, cached dynamic sleep data, or both.  If no cache type is specified, **all** is assumed.

### Command: traceable-functions<a name="command_traceable_functions"></a>

List all functions that can be specified as an argument to option ***--trcfunc***, ***--trcfrom***, or ***--trcapi***. 

Not all functions are enabled for tracing, and of those not all have been registered as valid arguments for these options.



