
## Instrumentation



### Option: ***--ddc***

If option ***--ddc*** is specified, **ddcutil** reports protocol errors that it detects.  These may reflect I2C bus errors, or deviations by monitors from the MCCS specfication.  Most I2C errors cause a retry.  Most monitors are very clean.  Some, particularly older monitors, are very dirty. 

### Option: ***--statistics*** &nbsp; [&lt;stats-type]

Can be written as ***--stats***.  This option causes **ddcutil** to report execution statistics.  It takes the following optional arguments: 

| Argument | Action 
|------------------|------------------------------------|
| tries            | Report retry statistics            |
| errors           | Report I2C/DDC error counts
| calls            | Report system call counts and time |
| elapsed          | Report elapsed time summary        |
| time             | Synonum for **elapsed**            |
| all              | Report all statistics (default)    |


***--stats calls*** implies ***--stats elapsed***
 
Statistics can be voluminous.  To see only the elapsed execution time, use argument ***ELAPSED*** (alternatively ***TIME***).
For example
~~~
$ ddcutil detect --stats elapsed
~~~

Note: ***--stats tries*** seperately reports multi-part read and multi-part write tries.  This reflects how statistics are implementated.
As a practical matter, no monitor has ever been seen that has features of type ***Table***, so the 
multi-part write statistics will always be zero.

### Option: ***--vstats***

Break down statistics by monitor.  Recognizes the same arguments as ***--stats***. 

### Option ***--istats ***

Report internal statistics as well.

### Option: ***--syslog*** &nbsp; [&lt;level]

Writes messages with a **ddcutil** log level at least as severe as the specified level to the system log. 

**ddcutil** log levels are **DEBUG, VERBOSE, INFO, NOTICE, WARN, ERROR" and "NEVER"  and roughly correspond to syslog severity levels.

Log level **NEVER** turns off writing to syslog.




