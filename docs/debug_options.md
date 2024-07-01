### Tracing

#### Option: ***--trace &lt;trace-group>***

Each function in the code base, other than utility functions, is assigned to a trace group. 
This option enables trace messages for a group.

Can be specified more than once to trace multiple groups.

#### Option: ***--trcfile  &lt;file name>***

Enable trace messages in a single file.  The argument is a simple file name, e.g. "ddc_services.c".

Can be specified more than once to trace multiple files.


#### Option: ***--trcfunc &lt;function name>***

Enable trace messages in a particular function.  

Can be specified more than once to trace multiple functions.


#### Option: ***--trcfrom &lt;function name>***

Enable trace messages in a particular function and in all functinons called by that function.

Can be specified more than once to trace multiple files.

#### Option: ***--timestamp***, ***--ts***

Prefix trace messages with the number of seconds since **ddcutil** startup 

#### Option: ***--wall-timestamp***, ***--wts***

Prefix trace messages with the current clock time. 

#### Option: ***--thread_id***, ***--tid***

Prefix trace messages with the the thread number.  This is the Linux thread id.

#### Option: ***--thread_id***, ***--tid***

Prefix trace messages with the the process number.  This is the Linux process id.

<!--
#### Option: ***--syslog***

Write trace messages to the system log as well as the currently active trace message destination (typically the terminal). 
-->

### Other Debug Options


#### Option: ***-excp***

Portions of the **ddcutil** code base use an exception-like mechanism that can record the individual errors that
contribute to a master error.  If this option is specified, the exceptions are displayed before conversion
to a single status code.

#### Option: ***--trace-to_syslog-only***<a name="option_trace_to_syslog_only"></a>

#### Option: ***--stats-to-syslog***<a name="option_stats_to_syslog"></a>

#### Option: ***--debug-parse***<a name="option_debug_parse"></a>

#### Option: ***--parse-only***<a name="option_parse_only"></a>

#### Option: ***--failsim***<a name="option_failsim"></a>

#### Option: ***--quickenv***<a name="option_quickenv"></a>

#### Option: ***--enable-mock-data***<a name=option_enable_mock_data></a>

#### Option: ***--force-null-msg-for-unsupported***<a name="option_force_null_msg_for_unsupported"></a>




