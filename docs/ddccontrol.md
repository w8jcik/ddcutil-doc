## Comparison with ddccontrol

The program **ddccontrol** appears to be unmaintained, and has been dropped from distributions.  It is brittle 
in ways that I surmise reflect the services availble at the time it was written.  In particular:  

- **ddcctontrol**, as it is normally built (based on ***configure*** options that control conditional compilation), programs the I2C devcies directly at the PCI level.  It has dedicated code for each of several video card interfaces (e.g. nvidia, sis, intel740, intell 810).
**ddcutil**, 
on the other hand, relies exclusively on the the **i2c-dev** userspace 
interface to I2C.  This should make it less fragile to video card variations.
- **ddccontrol** uses a monitor attribute database to interpret VCP feature codes.  With MCCS 
2.0 and greater, VCP feature code definitions are largely standardized.  **ddcutil** uses the 
MCCS specification to interpret VCP feature values, and considerable
effort has gone into understanding that specification, particularly the variation among versions.  Unlike **ddccontrol**, **ddcutil** makes
no attempt to interpret values for feature codes designated as manufacturer 
specific (E0..FF). 