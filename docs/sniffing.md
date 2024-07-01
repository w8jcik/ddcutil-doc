## Reverse Engineering Proprietary DDC Extensions

Newer monitors often have features that exceed the capabilities of the Monitor Control Command Set specification.
Usually, the monitor manufacturer provides a Windows program to control these features. 
Typically, the program controls the features using VCP feature codes in the Manufacturer Reserved range (xE0..xFF), 
or through extensions to the definition of an existing feature (e.g. X60 - Input Source).  

Beyond trial and error, the real solution to figuring out the DDC command sequences used is to sniff the I2C protocol on the cable connecting the monitor to a Windows computer and see what requests the manufacturer's program is issuing and what responses the monitor is sending. As a practical matter, this needs to be a HDMI or DVI connection. DisplayPort "multiplexes" the I2C signal over its AUX channel, and Type-C carries a version of DisplayPort, so the I2C signal cannot easily be watched on these cables. 

There is at least one inexpensive piece of hardware that can sniff the I2C signal, called [I2CDriver](https://www.crowdsupply.com/excamera/i2cdriver). 
Documentation is [here](https://i2cdriver.readthedocs.io/en/latest/).
I also found this [DVI/DDC breakout cable](https://www.totalphase.com/products/video-dvi/) on [closeout](https://www.totalphase.com/offer/accessories-sale-2019), which saved me an afternoon of soldering. The I2C signal is exposed on a 10 pin header (the kind motherboards use for an external COM port connection). To make the final piece of the connection, you could solder the appropriate lines of an old serial ribbon cable onto header leads for the I2C driver board.  

The provided Python software shows the captured bits and bytes of the I2C bus.  A simple program could decipher this into the DDC/CI protocol sequences.

 To avoid soldering, I was able to jerry-rig the connection using an old COM port ribbon cable with a DB-9 male connector and a DB-9 female chassis connector with pins on the back. Here's a picture. ![I2CDriver Cabling](i2cdriver.png "I2CDriver Cabling")
