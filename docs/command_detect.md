### Command **detect**

Syntax: **detect** &lt;options>

Scans the system for displays supporting the Monitor Control Command Set (MCCS), most commonly using DDC/CI but alternatively using USB for communication.
This command reports the results of the display search, which also is performed in the background as part of most **ddcutil** commands. 



The output varies by verbosity. 

Normal output: 

~~~ text
$ ddcutil detect
Display 1<
   I2C bus:             /dev/i2c-7   
   EDID synopsis:  
      Mfg id:           ACR
      Model:            Acer X243W
      Serial number:    LAG040064310
      Manufacture year: 2007
      EDID version:     1.3
   VCP version:         2.1

Display 2
   I2C bus:             /dev/i2c-8
   EDID synopsis:
      Mfg id:           DEL
      Model:            DELL P2411H
      Serial number:    F8NDP11G119U
      Manufacture year: 2011
      EDID version:     1.3
   VCP version:         2.1

Display 3
   I2C bus:             /dev/i2c-10
   EDID synopsis:
      Mfg id:           HWP
      Model:            HP Z22i
      Serial number:    CNC51301ZP
      Manufacture year: 2015
      EDID version:     1.3
   VCP version:         2.2

Display 4
   I2C bus:             /dev/i2c-14
   EDID synopsis:
      Mfg id:           DEL
      Model:            DELL U3011
      Serial number:    PH5NY2CIANXL
      Manufacture year: 2012
      EDID version:     1.4
   VCP version:         2.1

~~~

#### Option: ***--brief***, ***--terse***

Presents a summary report of each detected display, in an easily machine readable form. 

~~~ bash
$ ddcutil detect --brief
Display 1
   I2C bus:             /dev/i2c-7
   Monitor:             ACR:Acer X243W:LAG040064310

Display 2
   I2C bus:             /dev/i2c-8
   Monitor:             DEL:DELL P2411H:F8NDP11G119U

Display 3
   I2C bus:             /dev/i2c-10
   Monitor:             HWP:HP Z22i:CNC51301ZP

Display 4
   I2C bus:             /dev/i2c-14
   Monitor:             DEL:DELL U3011:PH5NY2CIANXL

~~~



#### Option:  ***--verbose *** 

Show additional information, including the EDID. 

~~~ text
$ ddcutil detect --verbose
Display 1
   I2C bus:             /dev/i2c-7
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      I2C address 0x37 (DDC) responsive:  true 
      /sys/bus/i2c/devices/i2c-7/name:   AMDGPU i2c bit bus 0x92
   EDID synopsis:
      Mfg id:           ACR
      Model:            Acer X243W
      Serial number:    LAG040064310
      Manufacture year: 2007
      EDID version:     1.3
      Product code:     0
      Binary sn:        1952453327 (0x746012cf)
      Extra descriptor: 
      Video input definition: 0x80 - Digital Input
      Supported features:
         DPMS active-off
         Digital display type: RGB 4:4:4
         Standard sRGB color space: False
      White x,y:        0.313, 0.329
      Red   x,y:        0.640, 0.330
      Green x,y:        0.300, 0.608
      Blue  x,y:        0.150, 0.060
      Extension blocks: 0
   EDID source: 
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 04 72 00 00 cf 12 60 74   .........r....`t
      +0010   2e 11 01 03 80 34 20 78 2a ef 95 a3 54 4c 9b 26   .....4 x*...TL.&
      +0020   0f 50 54 a5 4b 00 81 80 81 00 81 0f 95 00 95 0f   .PT.K...........
      +0030   a9 40 b3 00 01 01 28 3c 80 a0 70 b0 23 40 30 20   .@....(<..p.#@0 
      +0040   36 00 06 44 21 00 00 1a 00 00 00 fd 00 38 4c 1e   6..D!........8L.
      +0050   52 11 00 0a 20 20 20 20 20 20 00 00 00 fc 00 41   R...      .....A
      +0060   63 65 72 20 58 32 34 33 57 0a 20 20 00 00 00 ff   cer X243W.  ....
      +0070   00 4c 41 47 30 34 30 30 36 34 33 31 30 0a 00 68   .LAG040064310..h
   VCP version:         2.1
   Controller mfg:      Mstar
   Firmware version:    0.3
   Monitor returns DDC Null Response for unsupported features: false

Display 2
   I2C bus:             /dev/i2c-8
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      I2C address 0x37 (DDC) responsive:  true 
      /sys/bus/i2c/devices/i2c-8/name:   AMDGPU i2c bit bus 0x93
   EDID synopsis:
      Mfg id:           DEL
      Model:            DELL P2411H
      Serial number:    F8NDP11G119U
      Manufacture year: 2011
      EDID version:     1.3
      Product code:     41070
      Binary sn:        825309525 (0x31313955)
      Extra descriptor: 
      Video input definition: 0x80 - Digital Input
      Supported features:
         DPMS standby
         DPMS suspend
         DPMS active-off
         Digital display type: RGB 4:4:4
         Standard sRGB color space: False
      White x,y:        0.313, 0.328
      Red   x,y:        0.631, 0.351
      Green x,y:        0.334, 0.620
      Blue  x,y:        0.156, 0.051
      Extension blocks: 0
   EDID source: 
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 10 ac 6e a0 55 39 31 31   ..........n.U911
      +0010   04 15 01 03 80 35 1e 78 ea bb 04 a1 59 55 9e 28   .....5.x....YU.(
      +0020   0d 50 54 a5 4b 00 71 4f 81 80 d1 c0 01 01 01 01   .PT.K.qO........
      +0030   01 01 01 01 01 01 02 3a 80 18 71 38 2d 40 58 2c   .......:..q8-@X,
      +0040   45 00 13 2b 21 00 00 1e 00 00 00 ff 00 46 38 4e   E..+!........F8N
      +0050   44 50 31 31 47 31 31 39 55 0a 00 00 00 fc 00 44   DP11G119U......D
      +0060   45 4c 4c 20 50 32 34 31 31 48 0a 20 00 00 00 fd   ELL P2411H. ....
      +0070   00 38 4c 1e 53 11 00 0a 20 20 20 20 20 20 00 63   .8L.S...      .c
   VCP version:         2.1
   Controller mfg:      Mstar
   Firmware version:    1.1
   Monitor returns DDC Null Response for unsupported features: false

Display 3
   I2C bus:             /dev/i2c-10
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      I2C address 0x37 (DDC) responsive:  true 
      /sys/bus/i2c/devices/i2c-10/name:   AMDGPU i2c bit bus 0x95
   EDID synopsis:
      Mfg id:           HWP
      Model:            HP Z22i
      Serial number:    CNC51301ZP
      Manufacture year: 2015
      EDID version:     1.3
      Product code:     12428
      Binary sn:        16843009 (0x01010101)
      Extra descriptor: 
      Video input definition: 0x80 - Digital Input
      Supported features:
         DPMS active-off
         Digital display type: RGB 4:4:4
         Standard sRGB color space: False
      White x,y:        0.313, 0.329
      Red   x,y:        0.647, 0.339
      Green x,y:        0.307, 0.633
      Blue  x,y:        0.146, 0.056
      Extension blocks: 0
   EDID source: 
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 22 f0 8c 30 01 01 01 01   ........"..0....
      +0010   0d 19 01 03 80 30 1b 78 2e f8 55 a5 56 4e a2 25   .....0.x..U.VN.%
      +0020   0e 50 54 a1 08 00 d1 c0 81 c0 81 80 95 00 a9 c0   .PT.............
      +0030   b3 00 01 01 01 01 02 3a 80 18 71 38 2d 40 58 2c   .......:..q8-@X,
      +0040   45 00 dd 0c 11 00 00 1e 00 00 00 fd 00 32 4c 18   E............2L.
      +0050   5e 11 00 0a 20 20 20 20 20 20 00 00 00 fc 00 48   ^...      .....H
      +0060   50 20 5a 32 32 69 0a 20 20 20 20 20 00 00 00 ff   P Z22i.     ....
      +0070   00 43 4e 43 35 31 33 30 31 5a 50 0a 20 20 00 7c   .CNC51301ZP.  .|
   VCP version:         2.2
   Controller mfg:      Mstar
   Firmware version:    1.0
   Monitor returns DDC Null Response for unsupported features: false

Display 4
   I2C bus:             /dev/i2c-14
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      I2C address 0x37 (DDC) responsive:  true 
      /sys/bus/i2c/devices/i2c-14/name:   card0-DP-2
   EDID synopsis:
      Mfg id:           DEL
      Model:            DELL U3011
      Serial number:    PH5NY2CIANXL
      Manufacture year: 2012
      EDID version:     1.4
      Product code:     16485
      Binary sn:        1095653452 (0x414e584c)
      Extra descriptor: 
      Video input definition: 0xb5 - Digital Input (DisplayPort)
      Supported features:
         DPMS active-off
         Digital display type: RGB 4:4:4 + YCrCb 4:2:2
         Standard sRGB color space: True
      White x,y:        0.313, 0.329
      Red   x,y:        0.678, 0.309
      Green x,y:        0.210, 0.692
      Blue  x,y:        0.146, 0.055
      Extension blocks: 1
   EDID source: 
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 10 ac 65 40 4c 58 4e 41   ..........e@LXNA
      +0010   33 16 01 04 b5 40 28 78 3a 8d 85 ad 4f 35 b1 25   3....@(x:...O5.%
      +0020   0e 50 54 a5 4b 00 71 4f 81 00 81 80 a9 40 d1 00   .PT.K.qO.....@..
      +0030   d1 40 01 01 01 01 e2 68 00 a0 a0 40 2e 60 30 20   .@.....h...@.`0 
      +0040   36 00 81 91 21 00 00 1a 00 00 00 ff 00 50 48 35   6...!........PH5
      +0050   4e 59 32 43 49 41 4e 58 4c 0a 00 00 00 fc 00 44   NY2CIANXL......D
      +0060   45 4c 4c 20 55 33 30 31 31 0a 20 20 00 00 00 fd   ELL U3011.  ....
      +0070   00 31 56 1d 71 1c 00 0a 20 20 20 20 20 20 01 56   .1V.q...      .V
   VCP version:         2.1
   Controller mfg:      Mstar
   Firmware version:    1.5
   Monitor returns DDC Null Response for unsupported features: false
~~~

#### Options: ***--enable-usb***, ***--disable-usb***

This pair of options controls whether **ddcutil** searches for displays that use USB for communication.  The default is ***--disable-usb***. 

Although the overhead of searching for USB displays has a negligable effect on **ddcutil** performance, the benign error messages it can produce regarding permissions have proven distracting.
USB display support is rarely required, as most monitors use DDC/CI, not USB, for MCCS communication.


#### Option ***--async***<a name="option_async"></a>

If there are 3 or more monitors and option ***--async*** is specified, initial monitor DDC checks are performed in separate threads.
marginally improving startup time. (This option may be removed in a future release.)