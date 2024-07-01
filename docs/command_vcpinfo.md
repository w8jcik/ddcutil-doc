### Command **vcpinfo** 

Syntax: **vcpinfo** &lt;display-selection-options> &lt;other options> &lt;feature-code-or-group> 

This command reports **ddcutil**'s internal table of MCCS definitions.

Monitor selection options are detailed [here](display_selection.md).

Feature code/group specification is detailed [here](feature_selection.md)

If no feature code or group is specified, ***ALL*** is assumed.

If there is no definition for a particular feature code, nothing is output. 

(TO DO: should issue error message if a specific code is given, as opposed to a group, and no definition is found for the feature.)

Normal output for a feature definition includes:

- Feature code
- Feature name
- Feature description (will indicate if the feature name varies by MCCS version)
- MCCS versions in which the feature is defined
- Feature groups that include the feature
- MCCS Specification Groups (TO DO: move to verbose output)
- Feature attributes: RW/RO/WO and type, which may vary by MCCS version

#### Option: ***--verbose***

In addition to normal output, reports:

- For simple non-continuous features, the values defined in the MCCS specification and their names (per version).
- MCCS specification groups.  These are the sections of the MCCS specification in which the feature code appears.
(For some features, the specification group varies by MCCS version.)  This field is entirely informational. It was impleented to aid in comparing **dddcutil**'s
feature code definitions with the specification. 

####O Option: ***--brief***

Outputs just a single line containig the feature code and name.  Useful to just get a table of ids. 

#### Option ***--mccs***<a name="option_vcpinfo_mccs"></a>

Filters the information returned by **vcpinfo** to that for the specified MCCS version.  For example, 
~~~
$ ddcutil vcpinfo --mccs 2.1
~~~