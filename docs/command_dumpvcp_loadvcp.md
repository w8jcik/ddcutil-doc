### Commands ***dumpvcp*** and ***loadvcp***

These commands save and restore the current settings of color management related VCP features, i.e. those in group PROFILE.

Syntax: **dumpvcp** &lt;display-selection-options> dumpvcp [&lt;file name>]

If no file name is specified on **dumpvcp**, output is written to directory $HOME/.local/share/ddcutil.
VCP Feature values are written in the format described for the ***getvcp*** (--terse)[command_getvcp.md#getvcp_terse] option.


Syntax: **loadvcp** &lt;display-selection-options> &lt;file-name>

Normally, the display to which the settings are to be applied is taken from the VCP file. In rare cases, this will be insufficient to identify the display, in which the the display selection options are required.  If specified, the display selected must be consistent with the identifiers stored
in the VCP file.

### Option: **--noverify**

See [**setvcp** option --noverify](command_setvcp.md#setvcp_noverify)

### Option: **--mccs &lt;vcp version>**<a name="option_dumpload_mccs"></a>

When executing **dumpvcp**, emit output as per the specified MCCS (aka VCP) version, instead of the VCP version obtained from feature xDF (VCP Version).
This can affect whether a feature value is output as type TABLE.

### Option: **--verbose**

Display informational messages regarding verification.
