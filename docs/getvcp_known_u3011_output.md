## ddcutil getvcp

The following example shows the values of all VCP Feature Codes that **ddcutil** understands for a Dell U3011 monitor attached at bus /dev/i2c-6.

~~~text
$ ddcutil getvcp known --bus 6

VCP code 0x02 (New control value             ): One or more new control values have been saved (0x02)
VCP code 0x0b (Color temperature increment   ): Invalid value: 0
VCP code 0x0c (Color temperature request     ): 3000 + 2 * (feature 0B color temp increment) degree(s) Kelvin
VCP code 0x0e (Clock                         ): current value =    50, max value =   100
VCP code 0x10 (Brightness                    ): current value =    57, max value =   100
VCP code 0x12 (Contrast                      ): current value =    56, max value =   100
VCP code 0x14 (Select color preset           ): sRGB (sl=0x01)
VCP code 0x16 (Video gain: Red               ): current value =   100, max value =   100
VCP code 0x18 (Video gain: Green             ): current value =   100, max value =   100
VCP code 0x1a (Video gain: Blue              ): current value =   100, max value =   100
VCP code 0x1e (Auto setup                    ): Auto setup not active (sl=0x00)
VCP code 0x20 (Horizontal Position (Phase)   ): current value =    50, max value =   100
VCP code 0x30 (Vertical Position (Phase)     ): current value =    50, max value =   100
VCP code 0x3e (Clock phase                   ): current value =    31, max value =   100
VCP code 0x52 (Active control                ): Value: 0x14
VCP code 0x59 (6 axis saturation: Red        ): current value =    50, max value =   100
VCP code 0x5a (6 axis saturation: Yellow     ): current value =    50, max value =   100
VCP code 0x5b (6 axis saturation: Green      ): current value =    50, max value =   100
VCP code 0x5c (6 axis saturation: Cyan       ): current value =    50, max value =   100
VCP code 0x5d (6 axis saturation: Blue       ): current value =    50, max value =   100
VCP code 0x5e (6 axis saturation: Magenta    ): current value =    50, max value =   100
VCP code 0x60 (Input Source                  ): DisplayPort-1 (sl=0x0f)
VCP code 0x6c (Video black level: Red        ): current value =    50, max value =   100
VCP code 0x6e (Video black level: Green      ): current value =    50, max value =   100
VCP code 0x70 (Video black level: Blue       ): current value =    50, max value =   100
VCP code 0x87 (Sharpness                     ): current value =     8, max value =   100
VCP code 0x8a (Color Saturation              ): current value =    50, max value =   100
VCP code 0x90 (Hue                           ): current value =    50, max value =   100
VCP code 0x9b (6 axis hue control: Red       ): current value =    50, max value =   100
VCP code 0x9c (6 axis hue control: Yellow    ): current value =    50, max value =   100
VCP code 0x9d (6 axis hue control: Green     ): current value =    50, max value =   100
VCP code 0x9e (6 axis hue control: Cyan      ): current value =    50, max value =   100
VCP code 0x9f (6 axis hue control: Blue      ): current value =    50, max value =   100
VCP code 0xa0 (6 axis hue control: Magenta   ): current value =    50, max value =   100
VCP code 0xac (Horizontal frequency          ): 32864 hz
VCP code 0xae (Vertical frequency            ): 59.80 hz
VCP code 0xb2 (Flat panel sub-pixel layout   ): Red/Green/Blue vertical stripe (sl=0x01)
VCP code 0xb4 (Source Timing Mode            ): mh=0x00, ml=0x02, sh=0x00, sl=0x01
VCP code 0xb6 (Display technology type       ): LCD (active matrix) (sl=0x03)
VCP code 0xc0 (Display usage time            ): Usage time (hours) = 5199 (0x00144f) mh=0x00, ml=0x00, sh=0x14, sl=0x4f
VCP code 0xc6 (Application enable key        ): 0x45cc
VCP code 0xc8 (Display controller type       ): Mfg: Mstar (sl=0x05), controller number: mh=0x00, ml=0x94, sh=0x85
VCP code 0xc9 (Display firmware level        ): 1.5
VCP code 0xca (OSD                           ): OSD Enabled (sl=0x02)
VCP code 0xcc (OSD Language                  ): English (sl=0x02)
VCP code 0xd6 (Power mode                    ): DPM: On,  DPMS: Off (sl=0x01)
VCP code 0xdc (Display Mode                  ): Standard/Default mode (sl=0x00)
VCP code 0xdf (VCP Version                   ): 2.1
~~~