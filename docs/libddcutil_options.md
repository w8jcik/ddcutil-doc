## Shared Library Options

Generally speaking, any **ddcutil** option, other than those for display and feature selection, apply also to **libddcutil**.
They are processed when **libddcutil** is initialized.

Options can be passed to **libddcutil** in one of two ways:

- Using the [configuration file](config_file.md).
- Using API initialization function **ddca_init()**.

In addition to general **ddcutil** options (other than those for display and feature selection),
some options are recognized only by **libddcutil.**.  They are not recognized on the
**ddcutil** command line. 
(Bear in mind that the shared library is not used by the command line program **ddcutil**.)

### Option: ***--libddcutil-trace-file &lt;file name>***<a name="option_libddcutil_trace_file"></a>

**libddcutil** can redirect all output that would normally be sent to the terminal to a trace file instead,
enabling tracing when there is no terminal to which to write. 
This option specifies the file name. The file name specified can be either absolute or relative.
If relative, the directory is resolved as per the definition of XDG_STATE_HOME in the XDG desktop specification.
Typically, this is $HOME/.local/state/ddcutil.

### Option: ***--trcapi &lt;function name>***<a name="option_trcapi"></a>

Trace an API function and all functions called by that function.  

Can be specified more than once to trace multiple functions.

### Option: ***--profile-api*** <a name="option_profile_api"></a>

Report time consumed by API calls.

### Options: ***--enable-watch-displays***, ***--disable-watch-displays***<a name="option_watch_displays"></a>

Watch for display connection and disconnection. As of release 2.0, this option exists for development only, given 
the complexity of accurately detecting changes in monitor connection and state.  There is currently no way to
report changes using the API.

The default is ***--disable-watch-displays***.


