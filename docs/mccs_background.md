## Monitor Control Command Set

### MCCS Terminology

Monitor settings are referred to as Virtual Control Panel (VCP) features.
Features are numbered from 0..255, and by convention are referred to using the hex
representation, e.g. 1A for blue level.   

MCCS designates features as being one of three types:

- Continuous (C): Able to take any value up to some maximum
- Non-continuous (NC): Able to take only a designated set of values
- Table (T): Used for "raw" data such as a video LUT

This clean distinction broke down as the MCCS specification evolved.
Some features have changed category.  ddcutil distinguishes
between "simple" NC fields, which are encoded using only a single byte (in field SL) and for which there is a simple list of possible values,
and "complex" NC fields, where an algorithm must be applied to 
interpret the bytes of a feature value.  This is a ddcutil distinction and 
not part of the MCCS specification.

Most features are both readable and writable (RW).  Some, such as 
VCP Version (DF), are read-only (RO), while others, such as Restore
Factory Defaults (02) are write-only.

### MCCS Versions

- Version 1, September 1998  
Initial version. Seen only on very old monitors.

- Version 2.0, October 2003  
A major update to the standard.  It introduced "complex" NC features, in particular VCP Version (feature DF).

- Version 2.1, May 2005  
A minor update to the standard, including clarifications.   

- Version 3.0, July 2006  
Not upwardly compatible with Version 2.1.  Did not achieve industry acceptance.  I know of no monitors implementing MCCS 3.0. 

- Version 2.2, July 2009  
Extends version 2.1, incorporating most of the new features from Version 3.0 in a manner upwardly compatible with 2.1. 

- Version 2.2a, July 2011   
Minor revision to Version 2.2. 

Note that Version 2.2 was issued after Version 3.0,  Therefore, when **ddcutil** reports MCCS versions it orders them 1.0, 2.0, 2.1, 3.0, 2.2. 





