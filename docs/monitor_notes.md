## Notes on Specific Monitors

Information about specific monitors is now maintained on the
[ddcutil repo wiki](https://github.com/rockowitz/ddcutil/wiki), which any github user can modify. This page is no longer maintained.


The following list describes some monitors that have been tested or reported by users.  It highlights the variability in DDC implementation among monitors.

### AOC Q2775

VCP Version:   2.2  
Controller Manufacturer:  Mstar   
Controller Model:  mh=0xff, ml=0x16, sh=0x00  
Firmware version:  0.7  
Manufacture year: 2016  

Feature x0B: Color temperature increment:  100 deg Kelvin  
Feature x0C: Color temperature request: 100  
Caclculated color temperature = 3000 + 100 * 100 = 3000+10000 = 13000 deg Kelvin  (nonsensical)  

Capabilities string has controller manufacturer name in model() field.

### Apple Cinema Display A1082

Has both I2C and USB interfaces. 

VCP Version:  2.0  
Controller Manufacturer:   unspecified  
Controller Model:          unspecified    
Firmware version:          unspecified  
Manufacture year:          2005

I2C interface has more VCP features than USB interface. 

USB interface does not report VCP version (feature DF).

Able to read EDID over USB. 

Capabilities string does not begin and end with parentheses. 

### Asus MG279

The capabilities string is malformed.

### Asus PG329Q

VCP version 2.2
Controller Manufacturer: Novatek
Firmware version: 0.36
Manufacture year: 2020

The EDID contains neither an ASCII nor a binary serial number.

### Asus ROG PG279Q

VCP version:  2.2   
Controller Manufacturer:   manufacturer defined controller   
Controller Model:     
Firmware version: 5.57, 5.59
Maufacture year:  2017, 2018

Does not support VCP codes x0B and X0C. 

Does not support feature x60 - Input Source

Does not respond to getvcp of unsupported VCP codes by setting the unsupported feature flag in the GETVCP
response. Requests for unsupported features return garbage bytes, the requests fail by exceeding retries.

The output of the ***capabilities*** command does appear to accurately report which features are supported.

### Asus VE247

VCP version:  2.0   
Controller Manufacturer:   RealTek  
Controller Model: mh=x00, ml=x24, sh=x82     
Firmware version: 2.0   
Maufacture year:  2010

Responds to unknown VCP codes using the DDC NULL message instead of setting the ***Unsupported VCP code*** bit in the GETVCP response.

Sensible responses to VCP codes x0B and x0C. 

### Asus VS248

The value of feature x60 (Input Source) can be changed only from the currently active input.


### BenQ  GW2480

VCP Version: 2.1  
Controller Manufacturer: Mstar   
Controller Model:    mh=0x00, ml=0x58, sh=0x00  
Firmware version: 3.10  
Manufacture year: 2018

Color temperature increment (x0b) = 50 degrees Kelvin  
Color temperature request   (x0c) = 70  
Requested color temperature = (3000 deg Kelvin) + 70 * (50 degrees Kelvin) = 6500 degrees Kelvin

Brightness and possibly other color related features are disabled when
Dynamic Contrast Range (DCR) is enabled (using OSD).


### BenQ XL2411Z

VCP version:  2.2  
Controller Manufacturer: Mstar  
Controller Model: mh=x00 mh=x00, ml-x85, sh=x56  
Firmware version: 2.1   
Manufacture year: 2013


Feature x0B: Color temperature increment:  50 deg Kelvin  
Feature x0C: Color temperature request: 0  
Caclculated color temperature = 3000 + (0\*50) = 3000 deg Kelvin  (nonsensical)  

Responds to VCP feature x60 (Input Source) even though this is not documented in the capabilities string.

<a name="dell_aw3401dw"></a>
### Dell AW3418DW

VCP Version: 2.2  
Controller Manufacturer: manufacturer designed (sl=0xff)  
Controller Model: mh=0x01  ml=0x08 sh=0x04  
Firmware version: 4.133  
Manufacture year: 2017  

Feature x0B: Color temperature increment:  unsupported   
Feature x0C: Color temperature request: unsupported  

Reading unsupported feature 0x00 returns normally, with ml=ml=sh=sl=0.  The unsupported feature bit is not set.
Reading any other unsupported feature value results in an IOCTL failure with Linux errno=EIO (5).

<a name="dell_p2411"></a>
### Dell P2411

VCP version: 2.1   
Controller Manufacturer: Mstar  
Controller Model: mh=x00, ml=x00, sh=x56   
Firmware version:       1.1  
Manufacture year:       2011

Lots of I2C errors.  Heavily dependant on retries.  The CAPABILITIES command sometimes fails, even with maximum retries. 
More recently, works with nouveau driver, but fails with proprietary Nvidia driver. 

Reports VCP code 0B (color temperature increment) as 1 degree Kelvin, which makes the GETVCP response to VCP code 0C (color temperature
request) nonsensical.

<a name="dell_p2715q"></a>
### Dell P2715Q

VCP version: 2.1  
Controller Manufacturer: Mstar (sl=x05)  
Controller Model: mh=x00, ml=x00,x sh=x56   
Firmware version:       2.1  
Manufacture year:       2015

Feature x0B, Color temperature increment:   unsupported  
Feature x0C, Color temperature request:     unsupported

very clean, no  DDC retries needed

<a name="dell_s3422dwg"></a>
### Dell S3422DWG

VCP version: 2.1
Controller Manufacturer: RealTek (sl=0x09)
Controller Model: mh=0x00, ml=0x27, sh=0x17
Firmware version: 65.3
Manufacture year: 2021

Github user Noctivans has posted [extensive notes](https://github.com/rockowitz/ddcutil/issues/268) detailing what he discovered on how to
control Picture In Picture/Picture By Picture using undocumented features.


<a name="dell_u1905p"></a>
### Dell Ultrasharp 1905FP

Does not support DDC.

<a name="dell_u2413"></a>
### Dell Ultrasharp U2413

VCP version:   2.1  
Controller Manufacturer:  STMicroelectronics (sl=x0d)  
Controller Model:  mh=x00, ml=x93, sh=x01  
Firmware version:  2.1  
Manufacture year:  2013

This monitor has a hardware LUT, however LUT loading not supported using standard VCP feature codes.

Responds to VCP feature xc0 (Display Usage Time), but returns 0.

Color temperature:  
  color temp increment (feature 0B):  unsupported  
  color temp request (feature 0C):    unsupported  

<a name="dell_2407wfp"></a>
### Dell 2407wfp

VCP version: 2.0  
Controller Manufacturer:  Genesis  
Controller Model:  mh=0xff, ml=0xff, sh=0x4a  
Firmware version: 29.1

Cannot set brightness less than 30. **setvcp** values in the range 0..50 are mapped to the range 30..50
     
User eskampie created the following table. 

Requested setvcp value | &nbsp;Observed OSD value
-----------------------|-------------------
0  | &nbsp;30
10  | &nbsp;34
20  | &nbsp;38
30 |  &nbsp;42
40 |  &nbsp;46
50  | &nbsp;50
60  | &nbsp;60
70  | &nbsp; 70
80 |  &nbsp;80
90 |  &nbsp;90
100 |  &nbsp;100

<br>

<a name="dell_u2518d"></a>
### Dell U2518D

VCP Version: 2,1  
Controller Manufacturer: MStar  
Controller Model:   mh=0x00, ml=0x00, sh=0x56  
Firmware version:  2.1  
Manufacture year:  2018

Features 0B, 0C are unsupported.


Attempting to set feature x12 (contrast) less than 25 results in a value of 25. 
However, it is possible to set a lower contrast value in the OSD .  

Unlike the [Dell 2404wfp](http://www.ddcutil.com/monitor_notes#dell_2407wfp) feature x10 (brightness) can be set to any value between 0..100. 

Clean implementation, no I2c retries required.

<a name="dell_u3011"></a>
### Dell U3011

VCP version: 2.1   
Controller manufacturer:  Mstar   
Controller Model: mh=x00, ml=x94, sh=x85     
Firmware version: 1.5   
Manufacture year: 2012  

If a value is set using SETVCP, the new value takes effect and may appear in the on-screen display.   However, GETVCP (or the verification option) sometimes still retieves the old value, not the new one.  More specifically: 

Feature Name      | Code | Notes
------------------|------|---------
Contrast          | x12 | values below x25 take effect but the old value is returned
Video Gain: Red   | x16 | Values below 100 take effect, but the GETVCP reports 100
Video Gain: Green | x18 | Ditto
Video Gain: Blue  | x1a | Ditto
Display Mode      | xdc | 

<br>

Reports VCP code x0B (color temperature increment) as 1 degree Kelvin, which makes the GETVCP response to
VCP code x0C (color temperature request) nonsensical.

Color temperature:  
  color temp increment (feature 0B):  1 deg K  
  color temp request (feature 0C):    2
  calculated color temp = 3000 + (x0c_val * x0b_val) = 3000  + (1 * 2) = 3002 degrees Kelvin

<a name="dell_u3415w"></a>
### Dell Ultrasharp U3415W

VCP version: 2.1   
Controller manufacturer:  Realtek (sl=x09)      
Controller Model: mh=x00, ml=x11, sh=x11       
Firmware version: 65.1     
Manufacture year: 2015  

Neither feature 0B (color temperature increment) or 0C (color temperature request) are listed
in the capabilities string.
However, querying feature 0B does work.  Querying feature 0C fails.

Color temperature:  
  color temp increment (feature 0B):  100 deg K  
  color temp request (feature 0C):    query fails 

<a name="dell_u3818w"></a>
### Dell U3818DW

VCP version:             2.1  
Controller Manufacturer: RealTek  
Controller Model:        mh=0x00, ml=0x11, sh=0x11  
Firmware version:        65.1  
Manufacture year:        2017

Fails to set the Unsupported VCP Code bit in the **getvcp** response packet for unsupported features.

<a name="eizo_CG19"></a>
### Eizo Coloredge CG19

VCP version:  unreported   
Controller manufacturer and model: unreported (VCP feature code C8 unsupported)   
Firmware version: unreported (VCP feature code C9 unsupported)   

Reports EDID at I2C bus address x50.  Does not support DDC over I2C (bus address x37). 

Does implement MCCS over USB, and appears to conform to the USB Monitor Control Class Specification.
However, it appears that the more sophisticated monitor features such as LUT loading use manufacturer
specific USB HID reports. 

Unable to read EDID over USB, even though the HID Report for EDID can be located.

To set color related VCP feature values, Custom Color mode must be selected.   If set to sRGB or Calibrated, the SETVCP command
will appear to succeed, but will have no effect.  (In sRGB mode, Brightness can be set, in Calibrated mode it cannot.)

<a name="eizo_ev2785"></a>
### Eizo EV2785

Reports EDID ad I2c bus address x.50.  Does not support DDC/CI (bus address x37).

Uses USB for monitor control, but uses manufacturer-defined usage pages for communication, i.e. does not support the
***USB Monitor Control Class Specification***.

<!--
<a name="electriq_244KMHDR"></a>
-->
### Electriq 244MKHDR

VCP version:  2.2   
Controller manufacturer and model:  Realtek 0.1  
Firmware version: 0.1    

This is a marketer rebrand of a monitor created by some Chinese company, possibly Meirun.  The 3 character manufacturer id "WAM" is not registered.

The serial number reported is "demoset-1", 

The values reported by the monitor for features x10 (brightness) and x12 (contrast) are not those shown in the OSD. For details, see
[ddcutil issue 200: Electriq 24 inch monitor - brightness works but getvcp range doesn't match OSD range](https://github.com/rockowitz/ddcutil/issues/200).


Feature x60 (input source) reports invalid value x00 as the current input. 

Feature xCA (OSD/Button Control) reports that the OSD is disabled, even though it is enabled.



<a name="gateway_vx920"></a>
### Gateway Diamondtron VX920

VCP version:  Unspecified, implies 1.0   
Controller manufacturer and model:   Unknown (VCP feature code C8 unsupported)     
Firmware version:  Unknown (VCP feature code C9 unsupported)  


### Hanns G Hi221D

VCP version:  2.1   
Controller manufacturer:  Mstar   
Controller model:   mh=x00, ml=x92, sh=x00   
Firmware version:  0.6  

All GETVCP requests return a value, whether or not the feature is valid for the monitor.   The monitor never reports a feature as unsupported.

### HP LP1965

VCP version:   2.0  
Controller manufacturer:  Undocumented value:  x64  
Controller model:   mh=xff, ml=xff, sh=x86  
Firmware version: 0.3  
Manufacture year: 2007

Capabilities reports non-standard DDC command x4E.

Color temperature:  
  color temp increment (feature 0B):  50 deg K  
  color temp request (feature 0C):    70  
  calculated color temp = 3000 + (x0c_val * x0b_val) = 3000  + (70 * 50) = 6500 degrees Kelvin


### HP LP2475w

VCP version:             2.1  
Controller manufacturer: Genesis  
Controller model:        mh=xff   ml=xff  sh=x80  
Firmware version:        0.31   
Manufacture year:        2008  

Color temperature:  
  color temp increment (feature 0B):  3000 deg K  
  color temp request (feature 0C):    1  
  calculated color temp = 3000 + (x0c_val * x0b_val) = 3000  + (1 * 3000) = 6000 degrees Kelvin

LUT loading not supported using standard VCP feature codes.

Implements MCCS over USB as well as I2C.   However, the USB implementation appears to be non-standard.  It does not conform to the USB Monitor Control Class Specification. 

### HP LP2480zx

VCP version:   2.1   
Controller manufacturer:  Genesis   
Controller Model: mh=xff, ml=xff, sh=x80   
Firmware version:      0.139  
Manufacture year:      2008  

Heavily reliant on manufacturer specific VCP codes.  Most color related features, including loading the internal LUT, are not supported using
standard VCP feature codes.

Implements MCCS over USB as well as I2C.   However, the USB implementation appears to be non-standard.  It does not conform to the USB Monitor Control Class Specification. 

Sensible reponses to VCP codes 0B and 0C. 

Capabilities string does not match actual capabilities observed.  For example, capaibilites does not include VCP feature code x10, brightness, which is recognized by getvcp and setvcp.

Values in capabilities string for feature 60 is a mixture of hex and decimal values, i.e. 
~~~
 01 02 03 04 05 07 0C 13 14 15 17 1C
~~~
Should be:
~~~
 01 02 03 04 05 07 0C 0D 0E 0F 11 ??
~~~

Capabilities string does include cmds() segment listing supported commands, or model() segment listing model.

### HP w2207

VCP Version:               2.1  
Controller manufacturer:   Mstar    
Firmware Version:          3.5  
Manufacture year:          2007  

Color temperature increment (feature x0B) and request (feature x0C) result in a color temperature of 13,000 degress Kelvin

Responds with data to all VCP feature requests, never reports a feature as unsupported 

<a name="hp_zr2740w"></a>
### HP ZR2740w

VCP version:  2.2  

Implements only a few VCP codes. 

VCP feature codes 0B and 0C unsupported.

<a name="hp_z27n_g2">
### HP Z27n G2
</a>

VCP version:  2.2 

For VCP code 60 (input source), the second DisplayPort input is value 0x13, instead of the standard 0x10. 0x13 is reported accurately as the value to use in capabilities.

When using VCP code 60 to switch inputs, the value read by GetVCP only updates once the new source has a valid input signal. Until there is a valid signal, GetVCP continues to report the previously selected input.

<a name="iiyama_pl2492h"></a>
### Iiyama PL2492H

VCP Version:   2.2 (reported by capabilities string), 2.1 (reported by feature xdf)  
Controller Manufacturer:   Connexant  
Firmware version:   0.0  
Manufacture year:   2017

Color temperature increment (feature X0B):  100 degrees Kelvin  
Color temperature request (feature X0C):  results in a nonsensical color temperature 1300 (3000 + 100 * 100) degrees Kelvin  

Requires that a Save Current Settings packet (command ***scs***) be sent immediately after a Set VCP Feature packet (command ***setvcp***) to actually set the value. 

Never sets the unsupported feature bit in a VCP Feature Reply packet (command ***getvcp***).  The capabilities command must be checked to determine if a feature is supported.

<a name="iiyama_pl2779q"></a>
### Iiyama PL2779Q

VCP Version:    2.1  
Controller manufacturer:   unknown (feature code xC8 unsupported)  
Firmware version:          unknown (feature code xC9 unsupported)  
Manufacture year:          2013  

Color temperature increment (feature x0B) and request (feature x0C) result in a color temperature of 26,100 degress Kelvin

Does not include command x02 (VCP Response) or xE3 (Capabilities Reply) in its capabilities string, even though these are supported.

Responds to several VCP feature not listed in capabilities.

<a name="iiyama_pl2792q"></a>
### Iiyama PL2792Q

VCP Version:   2.2 (reported by capabilities string), 2.1 (reported by feature xdf)
Controller Manufacturer:   RealTek  (controller number mh=x00, ml=x27, sh=x85)  
Firmware version:   0.0  
Manufacture year:   2020

Color temperature increment (feature X0B):  100 degrees Kelvin  
Color temperature request (feature X0C):  results in a nonsensical color temperature of 1300 degrees Kelvin


Requires that a Save Current Settings packet (command ***scs***) be sent immediately after a Set VCP Feature packet (command ***setvcp***) to actually set the value. 

Never sets the unsupported feature bit in a VCP Feature Reply packet (command ***getvcp***).  The capabilities command must be checked to determine if a feature is supported.


### Lenovo ThinkVision X1

VCP Version: 2.1  
Controller manufactuer: Mstar  
Controller Model:       mh=0xff, ml=0x16, sh=0x00  
Firmware level          1.0  
Manufacture year:       2016

Color temperature:  
- color temperature increment (feature 0B):  100 deg Kelvin  
- color temperature request (feature 0C):    35      
- calculated color temperature:              3000 + (x0c_val * x0b_val) = 3000  + (35 * 100) = 6500 degrees Kelvin

Brightness (feature x10) cannot be changed when Dynamic Contrast Range (DCR) is enabled (using OSD).

Per user Janusz Kowalczyk, setting Dynamic Constrast Range and/or Low Blue light in the OSD disables various features in the OSD.
Presumably they cannot be changed by **ddcutil** either.

- Brightness and Contrast may or may not be disabled
- Color related settings may or may not be disabled
- Image scaling is disabled
- DisplayPort version selection is disabled 


### Lenovo L22e-20

VCP Version:     2.0   
Controller Manufacturer: Mstar  
Controller Model:        mh=0x00, ml=0x00, sh=0x00   
Firmware level  1.0  
Manuracture year:  2019

EDID has no serial number.

Color temperature:  
- color temperature increment (x0B):   100 degrees Kelvin  
- color temperature request:  (x0C)    0  
- calculated color temperature = 3000 + (0 * 100) = 3000 degrees Kelvin

Brightness and possibly other color related features are disabled when
Dynamic Contrast Range (DCR) is enabled (using OSD).


### LG 22MP48D-P

VCP Version: 2.1  
Manufacture year: 2016  
Controller manufacturer and model: MStar  
Firmware version: 5.0  

Features x0B and x0C are supported.  Calculated color temperature = 6500 degrees Kelvin.

EDID model name is generic "LG IPS FULLHD"

Never sets the Unsupported Feature bit in replying to a GETVCP request for an unsupported feature. 
Instead retuns a success response with all response bytes = 0. 


### LG 23EA73

Never sets the Unsupported Feature bit in replying to a GETVCP request for an unsupported feature. 
Instead retuns a success response with all response bytes = 0. 



### LG 27MU67

VCP Version:             2.1  
Manufacture year:        2015  
Controller manufacturer: Mstar  
Controller model:        mh=x00, ml=xff, sh=x00  
Firmware version:         3.7

Features X0B and X0C are not supported.  Cannot calculate color temperature.

No serial number in EDID.

When brightness (VCP feature x10) is changed, it reverts to the original value after approximately 1 second.
The same behavior is seen on Windows using ClickMonitorDDC and softMCCS, so this is clearly a monitor issue.
This problem is not observed for other features.

### LG 27MD5KL UltraFine

VCP Version: 2.2 (from the capabilities string)  
Manufacture year: 2020  
Controller manufacturer:  unknown (feature xc8 unsupported)  
Controller version:  unknown (feature xc8 unsupported)  
Firmware version: 3.4

The EDID model name is generic "LG UltraFine".

This 5K monitor appears to have been built with only Macs in mind. It has no buttons, and no on screen display (OSD).

Despite an extensive list of features in the capabilities string, **getvcp** for most returns with the unsupported feature
bit set in the response packet.  The only features in the capabilities string that can be read using **getvcp** are: 
0x10 (brightness),  xFD (manufacturer reserved), xFF (manufacturer reserved).
In particular, **getvcp** of feature xDF (VCP version) fails.  Per the MCCS specification, this 
feature must be supported as of MCCS 2.0. 

The following features unknown to MCCS are not listed in the capabilities string, but return a value for
**getvcp**: xD1, xD3,  xD5, xDD

This monitor also supports USB access to the Virtual Display Panel, but the only feature supported through this interface is 0x10 (brightness).

<a name="ig_29um69g"></a>
### LG 29UM69G

VCP Version:    2.1 
Manufacture year: 2019  
Controller manufacturer: RealTek  controller number: mh=0x00, ml=0xff, sh=0x00  
Firmware version: 3.24
EDID version: 1.3 

EDID model name is  "LG Ultrawide".  No ASCII "serial number".  (Binary serial number not checked)

Malformed capabilities string.  (The "cmds" segment is named "UM69cmds")

      Values (  parsed):
         0f: DisplayPort-1
         10: DisplayPort-2
         11: HDMI-1

         Has 1 HDMI, 1 DisplayPort, 1 USB-C Alt DP

Users report that it does not support DDC on the DisplayPort input.  (See issue [LG 29UM69G fails switching input](https://github.com/rockowitz/ddcutil/issues/100)) on github. 

Github user mbastian resported: 

> After more than four months of back and forth with German support I've got this:

> Our Korean software development takes note of reports of this kind, but this does not lead to customer feedback.
> Wwe do not see any need for further action here.

> At first they where denying that DDC was even supported, when I've pointed them to the manual and spec sheet then they insisted that it only applied to the OSD.
> Finally they escalated it.
> I've explained in detail what my problem is, asked if there is a firmware update or an undocumented ddc feature.... and all I got is a not very nice "Don't bother us again!"

And Github user AndyKassell reported this response from LG tech support:

> We have had a response from our Technical Team and they have advised that our monitors do not support DDC-CI officially. Some models are able to issue some commands such as brightness, contrast etc, but if there are issues with this, it would not be considered a fault as it is not guaranteed to work. I apologise for any frustration this causes. 

### LG 27BN88U  27UL850  27GN950-B 29UM69G 34Wk500-P 34WN80C-B 34UM88P 38WN95C

Users of all these recent LG displays report similar problems with VCP feature X60 (Input Source).
***getvcp 60*** always returns x00, which is not a valid value. 
***setvcp 60*** flashes the screen, sometimes opens the OSD, never sets the value


### LG 27UD88  29UMS7P

Users report this monitors do support featrue x60 


### LG HDR 4K

No model name string in EDID.  The binary model number is set (x7702 = 30471) 

No ASCII serial number in EDID.  Binary serial number is present

VCP version:   2.1   
Controller manufacturer:  MStar (sl=x05)  
Controller Model: mh=x0, ml=xff, sh=x00  
Firmware version:      3.4  
Manufacture year:      2019

Properly sets the Unsupport VCP Code bit in a GETVCP response.



### LG Ultrawide

VCP version:  2.1   
Controller manufacturer and model:  STK  
Firmware version:  0.1  
Manufacture year:  2014  

Feature codes are not separated by blanks in the vcp() section of the capabilities string.

No serial number in the EDID.

When responding to a Get VCP Feature request, the monitor never sets sets the Result Code field of the VCP Feature Reply to Unsupported VCP Code.
Instead, it always reports No Error.  
For unsupported VCP codes, all bytes in the response (MH, ML, SH, SL) are set to x00.



<a name="mitsubishi_rdt232wlm"></a>
### Mitsubishi RDT232WLM

VCP Version: 2.1  
Controller manufacturer and model: unreported (VCP feature code C8 unsupported)  
Firmware version: unreported (VCP feature code C9 unsupported)  
Manufacture year:  2010  


DDC/CI communication works for DVI connector.  Does not work on HDMI connector. 

DDC/CI communication requires signficant number of retries, and occasionaly fails because of the maximum
number of retries is exceeded.

Features x0B and X0C are not listed in the capabilities string.  The monitor replies
to getvcp requests for these features.
The response to feature X0B (color temperature increment) is 50 degrees Kelvin, which is reasonable. 
The response to feature X0C (number of color temperature increments) is 0, resulting in a temperature 
of 3000 degrees Kelvin, which is nonsensical.

Responds to getvcp for feature xA8, which  is not defined in the MCCS specification.

### Monoprice Dark Matter 40776

VCP Version: 2.1  
Controller manufacturer:   Mstar  
Controller model:  mh=x00, ml=xff, sh=x56  
Firmware version:  0.0  
Manufacture year:  2020  

This monitor has 2 HDMI inputs and 2 DisplayPort inputs.  The capabilities string reports that VCP feature x60 takes the values 
x11, x12, x0f, and x10, which per the MCCS spec are HDMI-1, HDMI-2, DisplayPort-1 and DisplayPort-2 respectively.  

However
the actual values, which are reported by **getvcp** and settable by **setvcp** are: 

- x05 - HDMI-1  
- x06 - HDMI-2  
- x07 - DisplayPort-1  
- x08 - DisplayPort-2 

A new input source can only be set when ***ddcutil setvcp 60*** is issued from the current input source. 

It is also the case that ***getvcp*** and other queries only work when issued on the current input source. 

The ASCII "serial number" in the EDID is blank, and the binary serial number is 0.  Consequently, 2 monitors manufactured at the same time will have
identical EDIDs, which will confuse software that depends on the EDID to uniquely identify a display. 



<a name="nec_lcd3090w"></a>
### NEC LCD3090WQXi

VCP Version 2.0  
Controller manufacturer and model: unreported (VCP feature code C8 unsupported)  
Firmware version: unreported (VCP feature code C9 unsupported)   
Manufactur year:  2012

Color temperature increment:  x0B  100 degress kelvin
Color temperature request     35 ->  3000 + 35\*100 = 6500



<a name="nec_p241w"></a>
### NEC P241W

VCP Version 2.0  
Controller manufacturer and model: unreported (VCP feature code C8 unsupported)  
Firmware version: unreported (VCP feature code C9 unsupported)   

The capabilities string has 2 additional sets of features, designated vcp_p02 and vcp_p10, in addition to the set specified in 
the vcp() field.  It also reports the commands xC2, xC4, xC6 and xC8, which are not part of the DDC/CI specification. 
Likely these commands are feature request/response pairs for vcp_p02 and vcp_p10. 

Does not use the usual VCP codes (e.g. x16/Red Gain) for color control.
Instead uses the 6 axis color control features x8c..xa0.

Has an internal hardware LUT, but it is not accessed using the MCCS defined features for LUT mannipulation. 

Uses a proprietary USB for communication, which does not support adhere to the ***USB Monitor Control Class Specification***. 


<a name="nec_pa241"></a>
### NEC PA241

VCP Version 2.0  
Controller manufacturer and model: unreported (VCP feature code C8 unsupported)  
Firmware version: unreported (VCP feature code C9 unsupported)   

Implements both I2C and USB interfaces.  

Makes heavy use of manufacturer specific VCP feature codes.  Also, ddcutil capabilities --verbose reports numerous manufacturer
specific DDC commands  The USB interface also makes heavy use of manufacturer specific usage codes.

Monitor must be set to ??? to allow changes. 

VCP code x0c (Color Temperature Request) returns 0, which is nonsensical. 

Does not use the usual VCP codes (e.g. x16/Red Gain) for color control.
Instead uses the 6 axis color control features x8c..xa0.

<a name="phillips_328p6vu"></a>
### Phillips 328P6VU

VCP Version: 2.2  (capabilities string)  
VCP Version: 3.0  (feature xDF)  
Controller manufacturer: Mstar (sl=0x05), controller number: mh=0xff, ml=0x16, sh=0x00  
Firmware version:  0.0  
Manufacture year: 2019  

The EDID contains a binary serial number, but does not contain an ASCII "serial number".

VCP code 0x0b (Color temperature increment   ): 100 degree(s) Kelvin  
When VCP code 0x14 (Select color preset) = User 1 (0x0b):  
VCP code 0x0c (Color temperature request     ): 3000 + 35 * (feature 0B color temp increment) degree(s) Kelvin -> 6500 degrees Kelvin  
This is sensible.

Feature xDF (VCP Version) incorrectly reports that the VCP version is 3.0.  

Problems with feature x60 (Input Source):  
- In MCCS version 3.0, feature x60 (Input Source) is of type Table, not Non-Continuous. Because of this ***getvcp*** incorrectly 
reads the feature value, and ***setvcp*** fails because it expects a value of type Table as input.  
- Feature value x00, which is not listed in any MCCS specification,
 represents the USB-C connector.

Feature xc0 (Display Usage Time) reports mh=ml=0xff, sh=sl=x00, which is nonsense.



### Phillips BDM3270

VCP Version: 2.2  
Controller manufacturer:  Novatek, model info: mh=xff, ml=xff, sh=x00  
Firmware version:  1.0  
Manufature year:  2016

Color temperature increment (VCP code x0B) = 50 degrees Kelvin (sensible)  
Color temperature request (VCP code x0C) yields a calculated color temperator of 3000+(70*50) = 6500 degrees Kelvin

### Phillips BDM4037U

VCP Version:  2.2  
Controller manufacturer:  Realtek (sl=x09), Controller model: mh=x00, ml=x00, sh=x00  
Firmware version 0.1  
Manufacture year:  2016  


  color temp increment (feature 0B):  100 deg K   
  color temp request (feature 0C):    70 -> 3000+(70*100) = 10000 deg K

DDC communication clean, no need for retries


### Samsung S32D850

Manufacture year:  2015  
VCP Version: 2.0  
Controller manufacturer:  Mstar, controller number: mh=x00, ml=x10, sh=x00  
Firmware version:         0.1  

Capabilites request returns a 0 length string.

Features x0B and X0C are unsupported.  Cannot calculate color temperature.

Has no serial number in EDID

### Samsung Syncmaster 213T 

Does not support DDC.


### Samsung Syncmaster 730B

VCP version:   Unspecified, implies 1.0   
Controller manufacturer and model:   Unknown (VCP feature code C8 unsupported)     
Firmware version:  Unknown (VCP feature code C9 unsupported)  

The capabilities string spcifies the values for non-continuous features in decimal rather than hexadecimal

VCP feature code

<a name="samsung_odyssey_g9"></a>
### Samsung LC49G95T Odyssey G9

VCP version reported by capabilitise string: 2.0  
VCP version reported by feature xdf:         2.1   
Controller manufacturer and model:  Novatek, model id:  mh=xff, ml=0xff, sh=0x00  
Firmware version: 0.0

Two users have reported that DDC/CI does not work over DisplayPort. In both cases they were using the amdgpu
video driver  This was true no matter which DisplayPort version was chosen in the OSD.
As of 12/2020, this is not fixed by the most recent monitor firmware update. User razamatan reports
that he was told in Samsung's chat support that DDC/CI is not implemented on DisplayPort.

The capabilites string reports an unrecognized command code, x4e

Feature x14 (Color Perset), x06 (Color Temperature Increament) and x06 (Color temperature requrest) appear consistent.
In one report:  
   VCP code 0x14 (Select color preset           ): 7500 K (sl=0x06)  
   Color temperature increment (x0b) = 50 degrees Kelvin  
   Color temperature request   (x0c) = 90  
   Requested color temperature = (3000 deg Kelvin) + 90 * (50 degrees Kelvin) = 7500 degrees Kelvin

The monitor has two DisplayPort inputs and one HDMI input.  However, the capabilities string reports valid values as being
as x01 - VGA and X03 - DVI-1. 

The following features are not declared in the capabiliteis string but were found by scanning:  
   Feature x52 - Active control  
   Feature xb2 - Flat panel sub-pixel layout  
   Feature xf4 - Manufacturer Specific  
   Feature xfe - Manufacturer Specific


<a name="samsung_u32h750"></a>  
### Samsung U32H750  

VCP version reported by capabilities string: 2.1,  
VCP version reported by feature xdf: 2.0  
Controller manufacturer and model:  Mstar, model id:  mh=x00, ml=0x10, sh=0x00  
Firmware version: 0.1  

This monitor is a MCCS disaster area.

The capabilities string returned by the monitor is malformed.  As a result, **ddcutil** cannot parse the "cmds" field.  

Features x0B and X0C are unsupported.  Cannot calculate color temperature.

No matter what the color mode (x14 Preset, xDC Display Mode) color settings (x16, x18, x1a) cannot be changed.  Brightness (x10) and contrast can be changed only in some modes. 

Values in the capabilities string correlate poorly with observed values. 
For example, according to the capaiblities string, feature x60 (Input source) has values x11, x12, and x0f, which the MCCS spec defines as HDMI-1, HDMI-2
and DisplayPort-1 respectively.  However, the observed values are x05, x06, and x0f, which the MCCS spec defines as Composite video 1, 
Composite video 2, and Displayport respectively. 



<a name="viewsonic_xg2703"></a>
### Viewsonic XG2703-GS

Manufacture year:  2016  
VCP Version:  2.2  
Controller manufacturer:  Manufacturer-designed (sl=xff)  
Firmware version:         1.14  

No serial number in EDID

For unsupported features, sometimes returns a well-formed reply with the "unsupported feature" flag not set.
More often, garbage is returned, resulting in the maximum number of retries.

Does not support feature x60 (input switching)

Features X0B and x0C are unsupported.  Cannot calculate color temperature.
