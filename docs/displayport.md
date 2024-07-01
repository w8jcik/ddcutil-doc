## DDC Over DisplayPort

Sometimes, DDC works over a HDMI or DVI connection but not over DisplayPort.  Occasionally the converse is true.
This section describes how DDC, or more precisely I<sup>2</sup>C, is communicated over DisplayPort, and may prove helpful when analyzing
DDC communication problems.

I<sup>2</sup>C is a simple serial protocol that uses 2 signaling lines, referred to as SCL and SDA, sometimes referred to as DDC clock and DDC data, along with their associated grounds.
These are pins 12/15 on a HD15 VGA connector, 6/7 on a DVI connector, and 15/16 on a HDMI connector. 

DVI and HDMI use the same protocol, TMDS (Transition Minimized Differential Signaling) for transmitting video information.   

While more recent versions of HDMI have additional capabilities, e.g. Ethernet and CEC, in terms of video and DDC communication DVI and HDMI are equivalent, 
except for bandwidth. It's possible to connect a DVI output on a host to a HDMI input on a monitor, or vice-versa,
using a simple DVI<->HDMI cable. (High bandwidth communication may require dual-link DVI and associated conversions, but that is tangential to the present discussion.)

DisplayPort, on ther other hand, uses its own packetized protocol for video.  It also has a secondary Auxilliary (AUX) channel (and pins) for non-video communication.  DisplayPort does not have the I<sup>2</sup>C SCL/SDA pins. Instead, the logical I<sup>2</sup>C signals are converted at each end into messages on the AUX channel. Put another way, 
DDC is implemented using  the AUX channel, but the AUX channel itself is not DDC. 
The "multiplexing" of I<sup>2</sup>C over the AUX channel is transparent to all but the lowest level of I<sup>2</sup>C software at each end.


It is possible to connect the DisplayPort output on a host to a DVI or HDMI input on a monitor using an "active", or sometimes a "passive", adapter. 

Most active adapters connect a DisplayPort output on the host to a DVI or HDMI input on the monitor. An active adapter converts the DisplayPort video
signal to the TMDS video signal used by DVI or HDMI. It also converts the I<sup>2</sup>C signals on the AUX channel to and from the physical SDA and SCL signals on 
the DVI/HDMI connector.  To the host, it appears that it is communicating with a DisplayPort input on the monitor. To the monitor it appears that 
it is communicating with a HDMI or DVI output on the host.

There are active adapters that convert a DVI/HDMI output on the host to a DisplayPort input on the monitor. These adapters are uncommon, and are 
much more expensive.  There do not appear to be any adapters that operate in both directions.

Active adapters will be labeled "Active".

If the video adapter implements Display Port Dual Mode, also known as DP++, then it is possible to use a "passive adapter" between the DisplayPort output on the host and the HDMI/DVI input on the monitor.

When the host detects that its DisplayPort output is connected to a HDMI/DVI monitor input instead of a DisplayPort input, 
it shifts to "dual mode", treating its DisplayPort connector as if it were a weird DVI/HDMI connector.  It uses TMDS for the video signal, and uses the AUX channel pins as simple SDA/SCL pins.
A "passive" adapter simply routes wires appropriately, and adjusts voltage levels.

Host DisplayPort connectors that support dual mode should be marked as such.  The marker is a D with a P inside, and two + signs to the left, one above the other. Most DisplayPort connectors on recent video cards are dual mode. Unfortunately, they are not always marked as such.  On the other hand, it is common for passive adapters to have the DP++ marker on the DisplayPort end.

<!--
Thunderbolt ports, which use the DisplayPort protocol for video signaling, support DP++ natively.
-->

A video adapter may have a limitation on the number of DisplayPort outputs that can operate in dual mode.  In that case, active adapters are required.

For futher details of DisplayPort, its <a href="https://en.wikipedia.org/wiki/DisplayPort">Wikipedia page</a> is an excellent resource.
