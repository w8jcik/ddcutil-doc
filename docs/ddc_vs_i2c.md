## DDC vs I<sup>2</sup>C

The relationship between DDC and I<sup>2</sup>C is a source of confusion.  Making matters worse, the terms are often used interchangably.
This section attempts to clarify the relationship.

I<sup>2</sup>C is a low level, slow speed specification of a 2 wire protocol for sending and receiving bytes. DDC/CI is layered on top of that protocol, describing particular sequences of bytes that are communicated between a master device (the host computer) and a slave device (a monitor) at address x37 on an I<sup>2</sup>C bus. By analogy,
DDC is as a subclass of superclass I<sup>2</sup>C, that is DDC obeys the
rules of I<sup>2</sup>C, but is more specific.  For example, only certain byte sequences are legal. SMBUS is another protocol based on I<sup>2</sup>C.  

For details on I<sup>2</sup>C, see the [References section](bibliography.md#i2c).

***ddcutil*** reads from and writes to the userspace ***/dev-i2c*** devices which present a file system like abstraction of an I<sup>2</sup>C bus.  It leaves
the bit-banging to the video device drivers, e.g. amdgpu, nouveau. 

Some time ago the Monitor Control Command Set (MCCS) specification took on the role of defining the virtual control panel (VCP) feature codes processed by DDC/CI.
The DDC/CI specification references the MCCS spec. Later, a protocol (the ***VESA UBS Monitor Control Class Specification***) was defined for carrying MCCS over USB, 
without use of DDC/CI.  Nonetheless this usage is also referred to casually as "DDC".
