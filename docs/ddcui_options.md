
### ddcui Command Line Options

Command line options of interest to general users:  

Options passed to **libddcutil**

| Option                      | Comments |
|-----------------------------|---------------|
| --sleep-multiplier=&lt;number>  |&nbsp; Sleep adjustment factor, can include decimal point, e.g. .1, 10
| --ddc                           |&nbsp;  Report DDC protocol and data errors to terminal |
| --nousb                         |&nbsp;  Skip detection of monitors that communicate using USB
| --udf                           |&nbsp;  Enable User Defined Features (Default)
| --noudf                         &nbsp;  Disable User Defined Features 
| --maxtries=&lt;comma separated list> |&nbsp;  Max try adjustment
| --trace=&lt;trace group name>   |&nbsp;  Group to trace
| --trcfile=&lt;file name>        |&nbsp;  File to trace
| --trcfunc=&lt;function name>    |&nbsp;  Function to trace (selected functions only)
 
<br>
Options both processed by **ddcui** and passed to **libddcutil**

| Option                      | Comments |
|-----------------------------|------------------------|
| --timestamp, --ts  &nbsp;           |&nbsp;  Preface trace messages with timestamp
| --thread-id, --tid &nbsp;         |&nbsp;  Preface trace messages with process id

<br>
The remaining options are processed solely by ***ddcui***.  They correspond to settings in the ***Options*** menu, or to the initial ***Model*** selection

Options that change the initial settings for the Feature Selection dialog:

| Option                      | Comments |
|-----------------------------|------------------------
| --feature-set=&lt;name>     |&nbsp;  Initial feature set to show
| --custom-feature-set="&lt;feature list>" |
| --only-capabilities         |&nbsp;  Restrict values to those in the capabilities string
| --all-capabilities          |&nbsp;  Always include values in capabilities string
| --show-unsupported          |&nbsp;  Report unsupported features in feature-set |
| --force-latest-nc-value-names | &nbsp; Take the names for NC values from the latest applicable MCCS spec |

<br>
Note that ***--only-capabilities*** and ***all-capabiites*** are applicable in  different contexts: 
- ***--only-capabilities*** applies when the feature group selected is ***MCCS***, ***COLOR***, ***MANUFACTURER***
- ***--all-capabilities*** applies when  the feature group is ***MCCS***

Features in the custom &lt;feature list> are specified as 2 byte hex numbers, with or without 
   leading "x" or trailing "h".  To specify more than one feature, separate the feature codes
   by commas or blanks, and enclose the entire list in quotation marks.  For example:
~~~
--feature-set "10 x12 14h"
~~~



<br>
Options that change the initial settings for the NC Values Source dialog: 

| Option                      | Comments |
|-----------------------------|------------------------
| --nc-values-source=&lt;source>   |&nbsp; Primary source of NC values: MCCS, Capabilities, Both

<br>
Options that change the initial settings for User Interface Options dialog: 

| Option                      | Comments |
|-----------------------------|------------------------|
| --require-control-key       |&nbsp;  Control key must be pressed to move slider





<br>
Initial display selection:

| Option                      | Comments |
|-----------------------------|------------------------
| --model=&lt;model name>    |


<br>
Other options processed solely by **ddcui**

| Option                      | Comments |
|-----------------------------|------------------------|
| --styles                    |&nbsp;  List available Qt styles
| --style=style-name          |&nbsp;  Use the specified Qt style
| --version, -V               |&nbsp;  Report version information




