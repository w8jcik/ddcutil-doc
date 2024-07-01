## DDC Null Message 

Per section 6.4 of the Display Data Channel Command Interface Standard, Version 1.1, October 29, 2004:

> A DDC NULL message from display to host is used in the following cases:  
> - To detect that the display is DDC/CI capable (by reading it at 0x6Fh I2C slave address)  
> - To tell the host that the display does not have any answer to give the host (not ready or not expected)  
> - The "Enable Application Report" has not been sent before using Application Messages

The second bullet point causes ambiguity:  

- In the context of a getting a VCP value or reading the capabilities string, a Null Message 
normally indicates that the display is not ready to process a DDC message.  The proper response in 
this case is to sleep for an extended period and retry.   
- Some monitors use the Null Message to indicate an 
unsupported feature, instead of returning a normal response with the Unsupported Feature bit set.
The proper response in this case is to not retry.  

However, it is still possible
that a monitor that users the Null Message to indicate an unsupported feature also on occasion uses
the Null Message to indicate an error.  The unfortunate consequence is that multiple retries will occur in cases where the Null Message is used to indicate VCP value not found, slowing down operation for those monitors.

DDC attempts to read VCP feature 0x00, which never exists, during the monitor detection phase to determine if a monitor uses the Null Message to indicated unsupported.  This is used to control whether 
a persistent Null Message response is interpreted by higher levels of **ddcutil** as an error or 
unsupported feature indication.