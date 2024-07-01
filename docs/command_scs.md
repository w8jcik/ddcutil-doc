### Command **scs**

Syntax:  **scs** &lt;display-selection-options>

Sends a DDC ***Save Current Settings*** request to the display, which saves the current monitor settings to the display's nonvolatile storage.

Some displays implement this request, others do not. (See the output of **ddcutil capabilities**.)
Even for most that do, the command has no apparent effect.

However, the command is required when settomg a featuer value for at least some Iiyama monitors.  Users of models 
[PL2492H](https://www.monitor_notes#iiyama-pl2942h) 
and [PL2792Q](https://www.ddcutil.com/monitor_notes#iiyama-pl2792q) 
have reported that ***setvcp*** commands do not actually change the value unless immediately followed by a ***scs*** command.  In that case, the ***setvcp*** command should
use the ***--noverify*** option.  Otherwise, ***setvcp*** internally performs a **getvcp** operation to check that the new value has actually been set, but retrieves the old value because **scs** has not occurred.  

For example: 
~~~
$ setvcp 10 85             # verifcation fails, the value is unchanged
$ setvcp 10 85 --noverify
$ scs
$ getvcp 10                # works after scs
~~~

