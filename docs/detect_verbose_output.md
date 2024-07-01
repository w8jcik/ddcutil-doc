##ddcutil detect

The following example shows output of **ddcutil detect --verbose** on a system with 3 monitors attached.

~~~text
Display 1
   I2C bus:  /dev/i2c-5
      DRM connector:                      card0-HDMI-A-1
      Driver:                             amdgpu
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      Is LVDS device:                     false
      /sys/bus/i2c/devices/i2c-5/name     AMDGPU DM i2c hw bus 3
      PCI device path:                    /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0/i2c-5
   EDID synopsis:
      Mfg id:               SAM - Samsung Electric Company
      Model:                U32H75x
      Product code:         3586  (0x0e02)
      Serial number:        HTOJ900304
      Binary serial number: 810634571 (0x30514d4b)
      Manufacture year:     2017,  Week: 37
      EDID version:         1.3
      Extra descriptor:        
      Video input definition:    0x80 - Digital Input
      Supported features:
         DPMS active-off
         Digital display type: RGB 4:4:4 + YCrCb 4:4:4
         Standard sRGB color space: False
      White x,y:        0.312, 0.329
      Red   x,y:        0.634, 0.341
      Green x,y:        0.312, 0.636
      Blue  x,y:        0.158, 0.062
      Extension blocks: 1
   EDID source: I2C
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 4c 2d 02 0e 4b 4d 51 30   ........L-..KMQ0
      +0010   25 1b 01 03 80 46 27 78 2a 5f b1 a2 57 4f a2 28   %....F'x*_..WO.(
      +0020   0f 50 54 bf ef 80 71 4f 81 00 81 c0 81 80 95 00   .PT...qO........
      +0030   a9 c0 b3 00 01 01 04 74 00 30 f2 70 5a 80 b0 58   .......t.0.pZ..X
      +0040   8a 00 b9 88 21 00 00 1e 00 00 00 fd 00 18 4b 1e   ....!.........K.
      +0050   5a 1e 00 0a 20 20 20 20 20 20 00 00 00 fc 00 55   Z...      .....U
      +0060   33 32 48 37 35 78 0a 20 20 20 20 20 00 00 00 ff   32H75x.     ....
      +0070   00 48 54 4f 4a 39 30 30 33 30 34 0a 20 20 01 67   .HTOJ900304.  .g
   VCP version:         2.2
   Controller mfg:      Mstar
   Firmware version:    0.1
   Monitor returns DDC Null Response for unsupported features: false

Display 2
   I2C bus:  /dev/i2c-6
      DRM connector:                      card0-DVI-D-1
      Driver:                             amdgpu
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      Is LVDS device:                     false
      /sys/bus/i2c/devices/i2c-6/name     AMDGPU DM i2c hw bus 4
      PCI device path:                    /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0/i2c-6
   EDID synopsis:
      Mfg id:               NEC - NEC Corporation
      Model:                P241W
      Product code:         26715  (0x685b)
      Serial number:        28107362UW
      Binary serial number: 16843009 (0x01010101)
      Manufacture year:     2012,  Week: 33
      EDID version:         1.3
      Extra descriptor:        
      Video input definition:    0x80 - Digital Input
      Supported features:
         DPMS standby
         DPMS suspend
         DPMS active-off
         Digital display type: RGB 4:4:4 + YCrCb 4:4:4
         Standard sRGB color space: False
      White x,y:        0.313, 0.329
      Red   x,y:        0.636, 0.335
      Green x,y:        0.297, 0.622
      Blue  x,y:        0.147, 0.074
      Extension blocks: 0
   EDID source: I2C
   EDID hex dump:
              +0          +4          +8          +c            0   4   8   c   
      +0000   00 ff ff ff ff ff ff 00 38 a3 5b 68 01 01 01 01   ........8.[h....
      +0010   21 16 01 03 80 34 20 78 ea f1 c5 a2 55 4c 9f 25   !....4 x....UL.%
      +0020   13 50 54 bf ef 80 81 c0 81 80 90 40 8b c0 95 00   .PT........@....
      +0030   a9 40 b3 00 d1 00 28 3c 80 a0 70 b0 23 40 30 20   .@....(<..p.#@0 
      +0040   36 00 06 44 21 00 00 1a 00 00 00 fd 00 32 55 1f   6..D!........2U.
      +0050   5c 11 00 0a 20 20 20 20 20 20 00 00 00 fc 00 50   \...      .....P
      +0060   32 34 31 57 0a 20 20 20 20 20 20 20 00 00 00 ff   241W.       ....
      +0070   00 32 38 31 30 37 33 36 32 55 57 0a 20 20 00 50   .28107362UW.  .P
   VCP version:         2.0
   Controller mfg:      Unspecified
Extended delay as recovery from DDC Null Response...
   Firmware version:    Unspecified
   Monitor returns DDC Null Response for unsupported features: false

Display 3
   I2C bus:  /dev/i2c-7
      DRM connector:                      card0-DP-1
      Driver:                             amdgpu
      I2C address 0x50 (EDID) responsive: true 
      Is eDP device:                      false
      Is LVDS device:                     false
      /sys/bus/i2c/devices/i2c-7/name     AMDGPU DM aux hw bus 0
      PCI device path:                    /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0/drm/card0/card0-DP-1/i2c-7
   EDID synopsis:
      Mfg id:               DEL - Dell Inc.
      Model:                DELL U3011
      Product code:         16485  (0x4065)
      Serial number:        PH5NY2CIANXL
      Binary serial number: 1095653452 (0x414e584c)
      Manufacture year:     2012,  Week: 51
      EDID version:         1.4
      Extra descriptor:        
      Video input definition:    0xb5 - Digital Input (DisplayPort), Bit depth: 10
      Supported features:
         DPMS active-off
         Digital display type: RGB 4:4:4 + YCrCb 4:4:4 + YCrCb 4:2:2
         Standard sRGB color space: True
      White x,y:        0.313, 0.329
      Red   x,y:        0.678, 0.309
      Green x,y:        0.210, 0.692
      Blue  x,y:        0.146, 0.055
      Extension blocks: 1
   EDID source: I2C
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
Extended delay as recovery from DDC Null Response...
   Controller mfg:      Mstar
   Firmware version:    1.5
   Monitor returns DDC Null Response for unsupported features: false


~~~
