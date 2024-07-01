## Performance and Tuning Options



### Sleep Management

The DDC/CI specification dictates that the host computer wait 40-200 ms (depending on operation) between sending a command to the monitor and reading the response.
When **ddcutil** adheres to the spec, it typically spends approximately 90% of its elapsed time sleeping.  Many monitors respond properly with much shorter waits.  On the other hand, there are monitors that require longer waits to avoid DDC/CI errors. 

There are two ways to adjust wait times: explicitly using option ***--sleep-multiplier***, and automatically using dynamic sleep adjustment.

<!--
option ***--enable-dynamic-sleep***. 
-->

### Option: ***--sleep-multiplier***<a name="option_sleep_multiplier"></a>

Option ***--sleep-multiplier*** applies a multiplication factor to the DDC/CI specified sleep times.
The multiplication factor is a floating point number. 
For example,  
~~~
--sleep-multiplier .5 
~~~
causes 40 ms waits to become 20 ms, and 
~~~
--sleep-multiplier 4
~~~
causes 40 ms waits to beome 160 ms. 

Note that ***ddcutil*** may automatically increase wait times when peforming retries.  Option ***--sleep-multiplier*** applies to the inital wait time.

Option ***--sleep-multiplier*** can significantly speed up **ddcutil** execution - some monitors have been seen to operate properly with a sleep-multiplier as low as .1, 

Decreasing the sleep multiplier increases the chance of DDC/CI communication failures, requiring retries.
Option ***---stats tries*** can help in picking an optimal value.


### Options: ***--enable-dynamic-sleep***, ***--disable-dynamic-sleep***<a name="options_enable_disable_dynamic_sleep"></a>

The default is ***--enable-dynamic-sleep***. 

<!--
This is a newer way to control sleep time and largely replaces the need to use option ***==sleep-multiplier***.
-->

The dynamic sleep algorithm automatically increases
the sleep-multiplier factor (as needed) and decrease the sleep multiplier factor 
(insofar as possible).  Data is maintained across program executions in file 
$HOME/.cache/ddcutil/stats.

Option ***--enable-dynamic-sleep*** or one of its variants such as ***--dsa*** turn it on (the default).
Option ***--disable-dynamic-sleep*** turns it off.

If both ***--sleep-multiplier*** and ***--dsa*** are specified, existing statistics are
discarded and the sleep algorithm restarts calculation with the specified sleep-multiplier value.
Therefore ***--sleep-multiplier*** should generally not be used along with ***--dsa***.

By default, existing statistics are retained even when dynamic sleep is disabled.
To discard cached sleep statistics at the start of program execution, use option ***--discard-cache dsa*** (alt. ***--discard-cache sleep***). 


### Retry Management

### Option: ***--maxtries***<a name="option_maxtries"></a>

**I2C** is an inherently unreliable protocol, requiring retry management.  

There are 3 kinds of operations in which retry is possible: 

- Write-only operation.  A request packet is written to the monitor with no subsequent read.  
  Used only to set a VCP feature value, and to execute command **scs** (Save Current Settings).
- Write-read operation.  A request packet is written to the monitor, followed by a reading a response packet.
  Most DDC protocol operations are of this type.
- Multi-part operation.  This is a "meta" operation, consisting of multiple 
  write-read or write-only operations. Used to query monitor capabilities, and for 
  querying and setting Table type VCP features. 

By default, the maximum number of tries for each operation type is:

- write-only operation:    4
- write-read operation:   10
- multi-part operation:    8

(Note that the number of retries is 1 less than the number of tries.)

Option ***--maxtries*** adjusts the maximum try counts.  Its argument
consists of 3 comma-separated values.  The following example sets the maximum 
try counts to 3 for write-only operations, 6 for write-read operations, and 9 
for multi-part operations.
~~~
--maxtries(3,6,9) 
~~~
A value of "0" or "." leaves the corresponding try count unchanged.   The following 
example changes only the maximum write-read try count:
~~~
--maxtries(0,7,.) 
~~~
The higest value to which a maximum try count can be set, is 15.

### Capabilities Caching

<!--
### Options: ***--enable-capabilities-cache***, ***--disable-capabilities-cache***<a name="option_enable_capabilities_cache"></a><a name="option_disable_capabilities_cache"></a> 
-->

**capabilities** is the most expensive **ddcutil** command in elapsed time.
It is also the most prone to failure on marginal I2C host/monitor 
connections, due the large number of I2C request/response operations involved.

The capabilities string is constant for any given monitor model.
Therefore it makes sense to cache the value.

Capabilities string caching is controlled by options ***--enable-capabilities-cache*** and ***--disable-capabilities-cache***.
The default is  ***--enable-capabilities-cache***.  Option ***--disable-capabilities-cache*** may be needed for certain edge cases.

The strings are saved in file **ddcutil/capabilities** within the XDG_CACHE_HOME directory.
Normally this is **$HOME/.cache/ddcutil/capabilities**.
This file can safely be erased if a stored capabilities string should become corrupted in some way.

Option ***--discard-cache capabilities***  erases the stored capabilities strings at the start of program execution.


### Additional Peformance Related Options


### Option: ***--discard-cache[s] [capabilities|dsa|all]***<a name="option_discard_cache"></a>

Discards the specified cache, or all caches, at the start of program execution.

Alternatively, command [**discard caches**](seconday_commands.md/#command_discard_caches) can be used to delete cache files.

### Option: ***--async***

Enable parallel display inspection if 3 or more monitors are detected.  See [detect command](command_detect.md#option_async).

The benefit of this option has proven marginal at best.  It may be eliminated in future releases.


#### Option: ***--noverify***<a name="setvcp_noverify"></a>

Skip checking that a monitor has properly processed a DDC ***Set VCP Feature*** request packet.
For details, see [setvcp command](command_setvcp.md#setup_noverify).

<!--
Additional performance related options exist but are experimental.  See the [Release Notes](release_notes.md).
-->