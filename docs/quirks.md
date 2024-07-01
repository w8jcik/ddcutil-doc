## Notes on Particular Configurations

This page contains notes on some particular configuration issues that have been reported by users.  They are too specific to put into the FAQ, but may be helpful to users trying to figure out why **ddcutil** does not work properly on a particular combination of computer, docking station, video card, driver, and monitor.

**Problem:** I2C communication not working using a DisplayPort output and DP to DVI (or HDMI) adapter.  Does work with DP to VGA adapter.  Radeon driver. RX 480 video card.

**Solution:**  The radeon ***hw_i2c*** parm was set to 1 in /etc/modprobe.d/radeon.conf.  The default is 0.  Setting the value back to 0 solved the problem.
Note that parm can also be specified as a boot option, radeon.hw_i2c=0.  (Reported by user sergio.)

