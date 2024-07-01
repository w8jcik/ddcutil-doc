## Virtual Machines

**ddcutil** can be built in a virtual machine.  However, when using the built-in video drivers, loading i2_dev does 
not create /dev/i2c-* devices, either on VirtualBox or VMware.   This is as expected, since DDC features affect the physical monitor, 
and the built-in virtualized video drivers do not implement I2C.

However, **ddcutil** will work in a virtual machine if using a normal video device driver with PCI passthrough, so that the device driver is 
actually controlling a real monitor.  

