### Command **getvcp**

Syntax: **getvcp** &lt;display-selection-options> &lt;other options> &lt;feature-codes-or-group> 

Monitor selection options are detailed [here](display_selection.md).

Feature code/group specification is detailed [here](feature_selection.md). Briefly, you can 
specify one or more feature codes or a feature group, but feature codes and groups cannot be combined.
For example, the folling commands are valid: 

~~~text
$ ddcutil getvcp 10 12
~~~
~~~
$ ddcutil getvcp color
~~~

(Note: The ability to specify multiple feature codes was added in [release 1.3.0](release_notes.md#release_notes_1.3.0))

If a feature group has been specified, there is normally no message if a feature is unsupported.
This rule is not absolute.  For some feature-groups it is appropriate to always report that a 
feature is unsupported. This is also affected by the ***--show-unsupported*** option described below.


<a id="getvcp_terse"></a>
#### Option: ***--terse***, ***--brief***

If ***--terse*** is specified on a getvcp command, output is in a format this is easily machine readable.
This format is also used by [dumpvcp](command_dumpvcp_loadvcp.md).  

The format depends on the feature type. 
In keeping with **ddcutil** 
practice, VCP feature codes are shown without a leading hex indicator even though they are 
hexidecimal values.

For a continuous VCP feature, both the current value (in decimal) and maximum value (in decimal) 
are shown.  Output takes the form:

<pre>
&nbsp;&nbsp;<em>VCP</em> feature-code <em>C</em> cur-value-decimal max-value-decimal
</pre>

For example:
~~~text
VCP 10 C 50 100
~~~

For a simple non-continuous VCP feature, i.e. one for which only the SL response byte is 
significant, the value is reported in hex.   Output takes the form:  

<pre>
&nbsp;&nbsp;<em>VCP</em> feature-code <em>SNC</em> hex-value
</pre>

For example:
~~~text
VCP 60 SNC x03
~~~

Complex non-continuous VCP features are ones for which more then 1 response byte is 
significant.  In that case all 4 bytes are reported in hex.  Output takes the form:

<pre>
&nbsp;&nbsp;<em>VCP</em> feature-code <em>CNC</em> mh-hex ml-hex sh-hex sl-hex
</pre>

For example:
~~~bash
VCP DF CND xff xff x02 x00
~~~

Table VCP features report the value as hexidecimal string.  Output takes the form:

<pre>
&nbsp;&nbsp;<em>VCP</em> feature-code <em>T</em> hex-string
</pre>

For example:
~~~bash
VCP 73 T x01020304
~~~

Finally, if a feature is not supported or some other error occurs, this may be reported
or there may simply be no output.  It is always reported for single feature ***getvcp*** requests. 
It is normally not reported if the request specifies a feature group, which defines a set of features. but see ***--show-unsupported***. 
If reported, output takes the form:

<pre>
&nbsp;&nbsp;<em>VCP</em> feature-code <em>ERR</em>
</pre>

For example:
~~~bash
VCP 84 ERR
~~~

#### Option: ***--verbose***

The internal representation (bytes MH, ML, SH, SL) is shown.  Unsupported features are always reported.


#### Option: ***--show-unsupported***


If querying a single feature, ***getvcp*** reports an error if the feature is unsupported or 
some other error occurs.  If querying a feature group, e.g. COLOR, output for invalid features is suppressd unless ***--verbose*** is specified.
The ***--show-unsupported*** option forces error messages in all cases. 

### Option: ***--mccs &lt;vcp-version>***<a name="option_getvcp_mccs"></a>

Interprets the feature value as per the specified VCP (aka MCCS) version, instead of using the VCP version obtained from VCP feature xDF.
