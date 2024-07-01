### USB-C DisplayPort Alt Mode

USB-C Alt Modes allow for signals other than USB to be carried on the pins of the USB-C connector. In particular, DisplayPort Alt Mode specifies how a DisplayPort signal is carried.

At its most basic, the Type-C connector becomes just a weird DisplayPort connector that uses several of the pins for DisplayPort signaling. 
This [Anand Tech article](https://www.anandtech.com/show/15752/displayport-alt-mode-20-spec-released) has a nice diagram
showing how the pins of the type-C connector are reallocated from use for high speed USB to being used for DisplayPort.
In particular, the SBU1 and SBU2 pins are assigned to carry the DP-AUX channel.
I<sup>2</sup>C communication, used by DDC/CI, is (one of several) signals that can be "multiplexed" on the AUX channel.
So a Type-C output from a host to a Type-C input on the monitor, or a Type-C output from a host to a DP input on the monitor should just work. 

A Type-C connector cannot implement DisplayPort Dual Mode.
It can only emit a DisplayPort signal, not a TDMS DVI or TDMS HDMI signal.
To communicate with a DVI or HDMI monitor input, an Active, not a Passive, adapter is required somewhere in the chain
of cables and adapters from the host device to the monitor. 
Note that a Type-C to HDMI connector of some sort, such as a simple dongle or a docking station, will perform Active signal conversion.

In all tested cases, if a Type-C to monitor connection displays a video signal, the I<sup>2</sup>C connection required by DDC/CI, works, 

<!--

video cad: Radeon WX5700
driver: AMDGPU

Type C Connector on Video Card

host type C connector <-> TypeC to Type-C cable <-> Type C input on monitor   untested

host type C  <-> TypeC to DP dongle  <-> DP-DP cable <-> DP monitor input                 works
host type C  <-> TypeC to DP dongle  <-> Startech MST HUB <- DP-DP cable -> DP monitor    works

host type C  <-> TypeC to DP dongle <-> DP-DVI active adapter <-> single link DVI cable <-> monitor DVI input   works
host type C  <-> TypeC to DP dongle <-> DP-DVI active adapter <-> dual link DVI cable   <-> monitor DVI input   works

host type C  <-> TypeC to DP dongle <-> DP-DVI passive adapter <-> single link DVI cable <-> monitor DVI input   no video
host type C  <-> TypeC to DP dongle <-> DP-DVI passive adapter <-> dual link DVI cable   <-> monitor DVI input   no video

REDO:
host type C <-> Type C to DP dongle <-> active DP-DVI <-> DVI-HDMI single link adapter <-> HDMI-HDMI cable <-> HDMI input    works  ? what is this?
host type C <-> Type C to DP dongle <-> active DP-DVI <-> DVI-HDMI dual link   adapter <-> HDMI-HDMI cable <-> HDMI input    works  ? what is this?
host type C <-> Type C to DP dongle <-> active DP-DVI <-> DVI-HDMI single link adapter <-> HDMI-HDMI cable <-> HDMI input    fails
host type C <-> Type C to DP dongle <-> active DP-DVI <-> DVI-HDMI dual link   adapter <-> HDMI-HDMI cable <-> HDMI input    fails


host type C <-> pink TypeC-HDMI dongle  <-> HDMI cable <-> HDMI input  ok

host type C <-> TypeC to DP dongle <-> DP-HDMI active cable <-> HDMI input
host type C <-> TypeC to DP dongle <-> DP-HDMI passive cable <-> HDMI input


Mini-DP Connector on Video Card

host mini DP <-> miniDP-DP dongle <-> DP-DP cable <-> monitor DP input   ok


host mini DP <-> mini-DP-DVI active dongle <-> DVI single link cable <-> DVI input     reduced video bandwidth, DDC/CI works
host mini DP <-> mini-DP-DVI active dongle <-> DVI dual link cable   <-> DVI input     ok

host mini-DP <-> mini-DP-DVI passive dongle <-> DVI single link cable <-> DVI input    works
host mini-DP <-> mini-DP-DVI passive dongle <-> DVI dual link cable   <-> DVI input    works


host mini DP <->Startech hub <-> DP-DP cable <-> monitor DP input  works
host mini DP <->Startech hub <-> DP-DVI active adapter   <-> DVI single link cable <-> DVI input on monitor LP2480zx EDID not found, Z22i ok
host mini DP <->Startech hub <-> DP-DVI active adapter   <-> DVI dual   link cable <-> DVI input on monitor ok
host mini DP <->Startech hub <-> DP-DVI passive adapter  <-> DVI single link cable <-> DVI input on monitor low bandwidth, EDID not found on LP2480zx, Z22i ok
host mini DP <->Startech hub <-> DP-DVI passive adapter  <-> DVI dual   link cable <-> DVI input on monitor ok

host mini DP <->Startech hub <-> DP-HDMI cable <-> HDMI input on monitor
                                                            Ivankey
                                                            Rosewill
                                                            other
host mini DP <-> mini-DP-DP dongle <-> DP-DVI active adapter <-> DVI single link cable <-> monitor DVI input   
             HP Z22i  DDC/CI works
             HP 2480zx single link detect fails
host mini DP <-> mini-DP-DP dongle <-> DP-DVI active adapter <-> DVI dual   link cable <-> monitor DVI input   
            HP 2480zx low bandwidth, DDC/CI ok

host mini DP <-> mini-DP-DP dongle <-> DP-DVI passive adapter <-> DVI single link cable <-> monitor DVI input  perhaps video lower resolution, DDC/CI works
            HP 2480zx single link, low bandwidth, ddcutil detect fails
            HP Z22i single link, low bandwitch DDC/CI ok

host mini DP <-> mini-DP-DP dongle <-> DP-DVI passive adapter <-> DVI dual   link cable <-> monitor DVI input  perhaps video lower resolution, DDC/CI works 
            HP 2480zx dual link low bandwidth, DDC/CI ok
           




host mini DP <-> miniDP-DP dongle 
host mini DP <-> miniDP-DP dongle <->DP-DVI active adapter <-> DVI-HDMI calble <-> HDMI input


-->