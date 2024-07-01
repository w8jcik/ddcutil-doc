## Color Management

A primary use for **ddcutil** is as part of color profile management.  A monitor calibration is meaningful only if the color settings on the monitor are the same as when the calibration was performed. 

Normally, monitor settings are changed by pressing buttons on the monitor bezel.  Depending on the monitor, the process ranges from annoying to excruciating.  **ddcutil** allows this process to be automated.

Command **ddcutil dumpvcp** saves the current color related monitor settings in a file, and **ddcutil loadvcp** restores them from a file. 

VCP feature codes in feature group **PROFILE** are, if implemented by the monitor, saved by **dumpvcp** and restored by **loadvcp**. 
These are:

- 10: Brightness
- 12: Contrast
- 13: Backlight control
- 14: Video gain: Red
- 16: Video gain: Green
- 18: Video gain: Blue
- 6B: Backlight level: White
- 6C: Video black level: Red
- 6D: Backlight Level: Red
- 6E: Video black level: Green
- 6F: Backlight level: Green
- 70: Video black level: Blue
- 71: Backlight Level: Blue

To see the list of feature codes saved and restored, you can also issue the command: 

~~~
ddcutil vcpinfo profile --terse
~~~

### Color Management Workflow Integration

Comments on integration with color management workflow. 

- The **ddcutil** graphical user interface **ddcui** can be run along side the monitor adjustment phase at the start of [DisplayCal](https://displaycal.net), 
making it easier to adjust the monitor. 

- C or C++ programs can use the API to access the dump/load services.

- Conceivably, profile related VCP values could be stored in a custom tag in monitor profile file instead of a separate file.



### Monitor Lookup Tables

Typically, monitor calibration creates a color lookup table that is loaded into the video card.  Some advanced monitors have an internal lookup table.   These monitors generally have a software development kit with an API for accessing the LUT, and typically thse SDKs support Linux either poorly or not at all.  

The MCCS specification defines a set of features for accessing the monitor's internal lookup table: 

- 73: LUT size
- 74: Single point LUT operation
- 75: Block LUT operation
- 76: Remote procedure call

These are what MCCS categorizes as "Table" type features.   Reading and writing table type features is implemented in **ddcutil**, but have not been tested for lack of a monitor to test with.

If a monitor were to implement these features, **ddcutil** should be able to load the LUT.  Unfortunately, no monitor with an internal hardware LUT that has yet been tested implements them.  In particular, none of the following monitors having a hardware LUT implement the VCP codes:

- HP Dreamcolor LP2480zx
- NEC PA241
- Eizo CG19 (has only USB interface)
- Dell U2413
