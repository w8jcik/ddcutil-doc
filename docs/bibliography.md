## Reference Documents

<!--   WRONG
.. index::
  :single: bibliography
-->

Searching for DDC documentation can be confusing. The DDC protocol evolved, being part of Access Bus at one
point, and shed various pieces, e.g. EDID, into separate specifications along the way.
See the [Wikipedia DDC page](https://en.wikipedia.org/wiki/Display_Data_Channel) for an overview of how it morphed. 

Most people will want to start with the following standards: 

- [VESA Display Data Channel Command Interface (DDC/CI) Standard Version 1.1, October 29, 2004](https://vesa.org/vesa-standards/) 
  Also available [here](https://milek7.pl/ddcbacklight/ddcci.pdf).

 - [VESA Monitor Control Command Set (MCCS) Standard Version 2.2a, July 13, 2011](https://vesa.org/vesa-standards/). The most recent standard. Also available [here](https://milek7.pl/ddcbacklight/mccs.pdf).


VESA Standards

- VESA Display Data Channel Command Interface Standard, Version 1. August 14, 1998. This is a predecessor to the current standard, DDC/CI 1.1.  The contents of this document sometimes clarify the current specification, but should not be regarded as definitive.

- [VESA Access.bus Specificaions Version 3.0, September 1995](https://vesa.org/vesa-standards/). Another predecessor specification.

- [VESA Display Data Channel Command Interface (DDC/CI) Standard Version 1.1, October 29, 2004](https://vesa.org/vesa-standards/) 
  Also available [here](https://milek7.pl/ddcbacklight/ddcci.pdf).

- VESA Monitor Control Command Set (MCCS) Standard Version 1, September 11, 1998.

- VESA Monitor Control Command Set (MCCS) Standard Version 2, October 17, 2003

- VESA Monitor Control Command Set (MCCS) Standard Version 2.1, May 28, 2005 

- VESA Monitor Control Command Set (MCCS) Standard Version 3, July 27, 2006.  MCCS 3.0 proved too great a departure from Version 2.1 and did not gain acceptance.

- [VESA Monitor Control Command Set (MCCS) Update Document for MCCS Standard Version 3, March 20, 2007](https://vesa.org/vesa-standards/)

- VESA Monitor Control Command Set (MCCS) Standard Version 2.2, January 19, 2009. MCCS 2.2 incorporated features from Version 3.0, but is upwardly compatible with 2.1

- [VESA Monitor Control Command Set (MCCS) Standard Version 2.2a, July 13, 2011](https://vesa.org/vesa-standards/). The most recent standard. Also available [here](https://milek7.pl/ddcbacklight/mccs.pdf).

- [VESA Enhanced Extended Display Identification Data Standard (E-EDID Standard), Relase A, Revision 1, February 9, 2000](https://vesa.org/vesa-standards/)

- [VESA Enhanced Display Data Channel (EDDC) Standard, Version 1.2, December 26, 2007](https://vesa.org/vesa-standards/)

- [VESA Enhanced Display Data Channel (E-DDC) Standard, Version 1.3, 11 September 2017](https://vesa.org/vesa-standards/)

- [VESA DisplayID Standard, Version 2.0, 11 September 2017](https://vesa.org/vesa-standards/)

- [DisplayID v2.0 Errata E6, Published 3/15/20](https://vesa.org/vesa-standards/)

- [DisplayPort Standard Version 1, Revision 1a 11 January 2008](http://file.yizimg.com/383992/2014090921252964.pdf) This early version of the DisplayPort standard is available on the Internet Archive's Wayback Machine.

- [DisplayPort Standard Version 1, Revision 2 January 5, 2010](https://ia601001.us.archive.org/8/items/DP1.2/DP-1.2.pdf) This early version of the DisplayPort standard is available on the Internet Archive's Wayback Machine. More recent versions of the standard appear to require VESA membership.

- [DisplayPort Interoperability Guideline Version 1.1, January 28, 2008](https://glenwing.github.io/docs/VESA-DP-Dual-Mode-1.1.pdf) Technical specification for DisplayPort Dual-Mode (DP++)

- [VESA Introduces Updated Dual-Mode Standard for Higher Resolution Interoperability with HDMI Displays](https://vesa.org/featured-articles/vesa-introduces-updated-dual-mode-standard-for-higher-resolution-interoperability-with-hdmi-displays/)

- [VESA Standards FAQ](https://vesa.org/vesa-standards/standards-faq/)

Many standards can be found [here](https://glenwing.github.io/docs/)

USB Implementers Forum Standards

- [USB Device Class Definition for Human Interface Devices Version 1.11, June 27, 2001](https://usb.org/document-library/device-class-definition-hid-111)

- [USB HID Usage Table Version 1.12, October 28,2004](https://usb.org/document-library/hid-usage-tables-112)

- [USB HID Usage Table Version 1.22, April 6,2021](https://usb.org/document-library/hid-usage-tables-122)

- [USB Monitor Control Class Specification Revision 1.0, January 5, 1998](https://usb.org/sites/default/files/usbmon10.pdf)

Many standards can be found [here](https://glenwing.github.io/docs)

### I<sup>2</sup>C<a name="i2c"></a>

As background, I<sup>2</sup>C is a low level, slow speed specification of a 2 wire protocol for sending and receiving bytes. DDC/CI is layered on top of that protocol, describing particular sequences of bytes that are communicated between a master device (the host computer) and a slave device (a monitor) at address x37 on an I<sup>2</sup>C bus. By analogy,
DDC is as a subclass of superclass I<sup>2</sup>C, that is DDC obeys the
rules of I<sup>2</sup>C, but is more specific.  For example, only certain byte sequences are legal. SMBUS is another protocol based on I<sup>2</sup>C.  

***ddcutil*** reads from and writes to the userspace **/dev/i2c*** devices which present a file system like abstraction of an I<sup>2</sup>C bus.  It leaves
the bit-banging to the video device drivers, e.g. amdgpu, nouveau. 

One source of confusion in DDC documentation is the specification of slave adddresses on the I<sup>2</sup>C bus.  Sometimes you'll see the DDC address documented as x37, other times as the pair x6e/x6f. Leaving aside the implementation of 10 bit I<sup>2</sup>C bus addresses (uncommon), I<sup>2</sup>C slave addresses are encoded in a single byte. The high order 7 bits are used for the address per se.  The low order bit is 0 for a write operation, 1 for a read operation. Much of the VESA documenation regards the entire 8 bit value as an address, which means address x37 becomes the pair x6e/x6f.  Similarly, the 7 bit address for the EDID is x50; regarded as an 8 bit address, the EDID address becomes xA0/xA1. ***ddcutil*** specifies I<sup>2</sup>C slave addresses in 7 bit form, as do utilities such as ***i2cdetect***, ***i2cget***, and ***i2cset***.

The following specifications and articles provide backgroud information about the I<sup>2</sup>C bus. 

- [Understanding the I<sup>2</sup>C Bus, Texas Instruments Application Report, SLVA702, June 2015](http://www.ti.com/lit/an/slva704/slva704.pdf)
- [HDCP and EDID Demystified](https://luxielectronics.com/attachments/File/HDCP_and_EDID_Demystified.pdf)
- [I<sup>2</sup>C-bus specification and user manual, Rev 6 - 4 April 2014](https://www.pololu.com/file/0J435/UM10204.pdf)
- [I<sup>2</sup>C Info -I<sup>2</sup>C Bus, Interface and Protocol](https://i2c.info)
- [I<sup>2</sup>C Bus Specification](https://i2c.info/i2c-bus-specification)
- [I<sup>2</sup>C Communication Protocol Tutorial](https://deepbluembedded.com/i2c-communication-protocol-tutorial-pic/)
- [I<sup>2</sup>C - What's That?](https://www.i2c-bus.org/)
- [I<sup>2</sup>C Primer](https://www.i2c-bus.org/i2c-primer)
- [Wikipedia](https://en.wikipedia.org/wiki/I<sup>2</sup>C)
- [7-bit, 8-bit, and 10-bit I<sup>2</sup>C Slave Addressing](https://www.totalphase.com/support/articles/200349176-7-bit-8-bit-and-10-bit-I<sup>2</sup>C-Slave-Addressing)
- [StackExchange thread on the confusion in I<sup>2</sup>C slave address representation](https://electronics.stackexchange.com/questions/151413/how-to-display-i2c-address-in-hex)
- [I<sup>2</sup>C Protocol Lecture Notes, Sudhanshu Janwadkar](https://www.slideshare.net/shudhanshu29/i2c-protocol-94259889)
- [Linux kernel documentation](https://www.kernel.org/doc/html/latest/i2c/index.html#introduction)
- [Using the I<sup>2</sup>C Bus, Robot Electronics](https://www.robot-electronics.co.uk/i2c-tutorial)
- [Definitions and Differences between I<sup>2</sup>C, Access.bus, and SMBUS](http://ww1.microchip.com/downloads/en/AppNotes/an617.pdf)Nazir Azzam, SMSC, Hauppage, NY
- [I<sup>2</sup>C-bus specification and user manual, Rev 6 - 4 April 2014](https://www.nxp.com/docs/en/user-guide/UM10204.pdf) Paywalled
- [I<sup>2</sup>C, SPI, I3C Interface Devices](https://www.nxp.com/products/interfaces/ic-spi-i3c-interface-devices:MC_41735)

SMBus is a related standard.  The following report can help in understanding I<sup>2</sup>C. 

- [SMBus Made Simple, Texas Instruments](https://www.ti.com/lit/an/slua475/slua475.pdf)

<!--
*** Cabling<a name=cabling"></a>

Users sometimes encounter problems with I<sup>2</sup>C communication over DisplayPort. The following articles may be of help.
-->

Wikipedia has several high quality pages on related specifications:  They may be helpful when trying to figure out why DDC communication is not working: 

- [DisplayPort](https://en.wikipedia.org/wiki/DisplayPort)
- [HDMI](https://en.wikipedia.org/wiki/HDMI)
- [Thunderbolt](https://en.wikipedia.org/wiki/Thunderbolt_\(interface\))
- [USB-C](https://en.wikipedia.org/wiki/USB-C). See in particular section [Hardware Support](https://en.wikipedia.org/wiki/USB-C#Hardware_support) for a discussion of ***USB-C Alternate Mode***
- [Display Data Channel](https://en.wikipedia.org/wiki/Display_Data_Channel) Has a useful summary of the relationships, evolution, and deprecation among releated standards, 

Documentation on DP <-> HDMI/DVI conversion:

- [Dell HDMI to DisplayPort Conversion Information](https://www.dell.com/support/kbdoc/en-hn/000125170/hdmi-to-displayport-dp-conversion-information)
- [Barco DisplayPort to HDMI Conversion: should I use an active or passive adapter](https://www.barco.com/en/news/displayport-to-hdmi-conversion-should-i-use-an-active-or-passive-adapter)
- [DisplayPort->HDMI dongles/adapters - active vs passive revisited](https://dancharblog.wordpress.com/2014/10/07/displayport-hdmi-dongles-active-vs-passive-re-visited/)
- [Tripp-Lite What's the Difference Between Passive and Active DisplayPort Adapters](https://blog.tripplite.com/whats-the-difference-between-passive-and-active-displayport-adapters)
- [VESA DisplayPort Interoperatility Guideline Version 1.1, January 28, 2008](https://glenwing.github.io/docs/VESA-DP-Dual-Mode-1.1.pdf)

<a name="biblio_type_c"></a>
Type C Connectors

- [Benq: USB C Introduction](https://www.benq.com/en-us/knowledge-center/knowledge/usb-c-introduction-what-is-dp-alt-mode.html)
- [TotalPhase: How DisplayPort Alt Mode is enabled over a USB Type-C Cable](https://www.totalphase.com/blog/2019/11/how-displayport-alt-mode-is-enabled-over-a-usb-type-c-cable/)
- [AnandTech: DisplayPort Alt Mode 2.0 Spec Releaaed](https://www.anandtech.com/show/15752/displayport-alt-mode-20-spec-released) Has a diagram of pin usage
- [Guide to USB-C Pinout and Features](https://www.allaboutcircuits.com/technical-articles/introduction-to-usb-type-c-which-pins-power-delivery-data-transfer/) 
  All About Circuits web site, December 10 2018
- [Wikipedia USB-C page](https://en.wikipedia.org/wiki/USB-C)
- [VESA DisplayPort Alternate Mode on USB Type-C Technical Overview](https://manualzz.com/doc/25713109/vesa-displayport-alternate-mode-on-usb-type-c) Jim Choate, VESA 
  Compliance Program Manager, September 27-28, 2016
- [VESA DisplayPort Alt Mode for USB Type-C Standard Feature Summary, Sept 22, 2014](http://www.vesa.org/wp-content/uploads/2014/09/DP-Alt-Mode-Overview-for-VESA-v1.pdf)  PowerPoint presentation
- [VESA - DisplayPort Alternate Mode on USB-C](https://www.usb.org/sites/default/files/D2T1-4%20-%20VESA%20DP%20Alt%20Mode%20over%20USB%20Type-C.pdf) Conference presentation, Jim Choate, 
  VESA Compliance Program Manager, USB Developer Days 2019, November 10, 2019
- [HDMI Alt Mode USB Type-C](https://www.hdmi.org/spec/typec) A technical description of the spec that had essentially no adoption.  Includes pin usage.
- [RIP HDMI Alt Mode, we hardly new ye](https://arstechnica.com/gadgets/2023/01/hdmi-to-usb-c-spec-axed-as-displayport-alt-mode-reigns-supreme/) Ars Technica, 1/12/2023
  Type-C HDMI alt mode never saw adoption.  What exist are Type-C DisplayPort alt mode cables, etc. that have circuitry to convert the DisplayPort signal to HDMI.
- [HDMI licensing administrator says the obscure HDMI Alt Mode specs are dead](https://www.techspot.com/news/97262-hdmi-licensing-administrator-obscure-hdmi-alt-mode-specs.html) Another article
  pointing out that USB-C HDMI Alt Mode devices don't exist.  What exist are Alt-Mode DisplayPort cables/connectors, which convert DisplayPort to HDMI. That is, the device with the Type-C connector
  emits DisplayPort signaling, not HDMI signaling.  The DisplayPort to HDMI conversion is performed by circuitry in the cable/connector.
- [Enable Desktop Mode (HDMI Alt Mode) in kernel/bootloader or with custom ROM](https://forum.xda-developers.com/t/enable-desktop-mode-hdmi-alt-mode-in-kernel-bootloader-or-with-custom-rom.4398501/)
  xda.developers.com forum discussion.  A discussion of why you (usually) can't get video output from a cell phone Type-C connector. Note that the mention of "HDMI Alt Mode" in the title is misleading.
- [DisplayPort over USB-C](https://www.dell.com/support/kbdoc/en-us/000141328/displayport-over-usb-type-c) Dell Knowledge Base Article 
- [What's up with DisplayPort over USB C](https://superuser.com/questions/1192638/whats-up-with-hdmi-and-displayport-over-usb-c) superuser.com, asked 3/2017, last modified 10/2020
- [Are all USB Type-C-to-HDMI cables active?](https://superuser.com/questions/1696232/are-all-usb-type-c-to-hdmi-cables-active) superuser.com asked 12/2020, last modified 12/2020
- [Add USB-C with DisplayPort-alt-mode to your PC](https://dancharblog.wordpress.com/2020/07/20/add-usb-c-with-dp-alt-mode-to-your-desktop-pc/) A nice overview of devices for adding a Type-C connector supporting DisplayPort Alt Mode to a PC whose motherboard or video card does not have such a connector.
- [Bi-directional DisplayPort->UDB-C, HDMI->USB-C, and HDMI->DP Cables](https://dancharblog.wordpress.com/2020/05/10/bi-directional-usbc-dp-cables/) Another nice overview of available hardware from Dan S. Charlton

DisplayLink

A proprietary method of sending video over a USB connection.  Not to be confused with DisplayPort. Does not support DDC/CI.

[DisplayLink vs Alt Mode Video Output](https://kb.plugable.com/docking-stations-and-video/chipset-modal)

Monitor Firmware

- [MonitorDarkly](https://github.com/RedBalloonShenanigans/MonitorDarkly)

DisplayPort Multi-Stream Transport (MST) Issues

- [DRM path: Update MST First Slot Information Based on Encoding Format](https://www.mail-archive.com/dri-devel@lists.freedesktop.org/msg370838.html)

More generally, search www.mail-archive.com, particularly dri-devel

- [Same MST display appears as 2 /dev/i2c devices](https://gitlab.freedesktop.org/drm/intel/-/issues/3605#note_1409446) A long thread on freedesktop.org
regarding MST I<sup>2</sup>C MST implementation that I initiated.  Includes comments from i915 developers regarding the complexity of the problem.

Miscellaneous

- [EDID and Monitor Connection](https://www.reddit.com/r/Monitors/comments/k18mbf/comment/gdms313/?context=3) A good explanation
the EDID readability for connected monitors that are powered off.  In this case, the EDID is always readable if a monitor is connected using DVI and HDMI, but may or may not be readable when connected using DisplayPort.

