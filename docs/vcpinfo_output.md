##ddcutil vcpinfo


Command **ddcutil vcpinfo --brief**  provides a quick list of all features defined in the Monitor Control Command Set that **ddcutil** knows about.

~~~plaintext
$ ddcutil vcpinfo --brief
VCP code: 01: Degauss
VCP code: 02: New control value
VCP code: 03: Soft controls
VCP code: 04: Restore factory defaults
VCP code: 05: Restore factory brightness/contrast defaults
VCP code: 06: Restore factory geometry defaults
VCP code: 08: Restore color defaults
VCP code: 0A: Restore factory TV defaults
VCP code: 0B: Color temperature increment
VCP code: 0C: Color temperature request
VCP code: 0E: Clock
VCP code: 10: Brightness
VCP code: 11: Flesh tone enhancement
VCP code: 12: Contrast
VCP code: 13: Backlight control
VCP code: 14: Select color preset
VCP code: 16: Video gain: Red
VCP code: 17: User color vision compensation
VCP code: 18: Video gain: Green
VCP code: 1A: Video gain: Blue
VCP code: 1C: Focus
VCP code: 1E: Auto setup
VCP code: 1F: Auto color setup
VCP code: 20: Horizontal Position (Phase)
VCP code: 22: Horizontal Size
VCP code: 24: Horizontal Pincushion
VCP code: 26: Horizontal Pincushion Balance
VCP code: 28: Horizontal Convergence R/B
VCP code: 29: Horizontal Convergence M/G
VCP code: 2A: Horizontal Linearity
VCP code: 2C: Horizontal Linearity Balance
VCP code: 2E: Gray scale expansion
VCP code: 30: Vertical Position (Phase)
VCP code: 32: Vertical Size
VCP code: 34: Vertical Pincushion
VCP code: 36: Vertical Pincushion Balance
VCP code: 38: Vertical Convergence R/B
VCP code: 39: Vertical Convergence M/G
VCP code: 3A: Vertical Linearity
VCP code: 3C: Vertical Linearity Balance
VCP code: 3E: Clock phase
VCP code: 40: Horizontal Parallelogram
VCP code: 41: Vertical Parallelogram
VCP code: 42: Horizontal Keystone
VCP code: 43: Vertical Keystone
VCP code: 44: Rotation
VCP code: 46: Top Corner Flare
VCP code: 48: Top Corner Hook
VCP code: 4A: Bottom Corner Flare
VCP code: 4C: Bottom Corner Hook
VCP code: 52: Active control
VCP code: 54: Performance Preservation
VCP code: 56: Horizontal Moire
VCP code: 58: Vertical Moire
VCP code: 59: 6 axis saturation: Red
VCP code: 5A: 6 axis saturation: Yellow
VCP code: 5B: 6 axis saturation: Green
VCP code: 5C: 6 axis saturation: Cyan
VCP code: 5D: 6 axis saturation: Blue
VCP code: 5E: 6 axis saturation: Magenta
VCP code: 60: Input Source
VCP code: 62: Audio speaker volume
VCP code: 63: Speaker Select
VCP code: 64: Audio: Microphone Volume
VCP code: 66: Ambient light sensor
VCP code: 6B: Backlight Level: White
VCP code: 6C: Video black level: Red
VCP code: 6D: Backlight Level: Red
VCP code: 6E: Video black level: Green
VCP code: 6F: Backlight Level: Green
VCP code: 70: Video black level: Blue
VCP code: 71: Backlight Level: Blue
VCP code: 72: Gamma
VCP code: 73: LUT Size
VCP code: 74: Single point LUT operation
VCP code: 75: Block LUT operation
VCP code: 76: Remote Procedure Call
VCP code: 78: Display Identification Operation
VCP code: 7A: Adjust Focal Plane
VCP code: 7C: Adjust Zoom
VCP code: 7E: Trapezoid
VCP code: 80: Keystone
VCP code: 82: Horizontal Mirror (Flip)
VCP code: 84: Vertical Mirror (Flip)
VCP code: 86: Display Scaling
VCP code: 87: Sharpness
VCP code: 88: Velocity Scan Modulation
VCP code: 8A: Color Saturation
VCP code: 8B: TV Channel Up/Down
VCP code: 8C: TV Sharpness
VCP code: 8D: Audio mute/Screen blank
VCP code: 8E: TV Contrast
VCP code: 8F: Audio Treble
VCP code: 90: Hue
VCP code: 91: Audio Bass
VCP code: 92: TV Black level/Luminesence
VCP code: 93: Audio Balance L/R
VCP code: 94: Audio Processor Mode
VCP code: 95: Window Position(TL_X)
VCP code: 96: Window Position(TL_Y)
VCP code: 97: Window Position(BR_X)
VCP code: 98: Window Position(BR_Y)
VCP code: 99: Window control on/off
VCP code: 9A: Window background
VCP code: 9B: 6 axis hue control: Red
VCP code: 9C: 6 axis hue control: Yellow
VCP code: 9D: 6 axis hue control: Green
VCP code: 9E: 6 axis hue control: Cyan
VCP code: 9F: 6 axis hue control: Blue
VCP code: A0: 6 axis hue control: Magenta
VCP code: A2: Auto setup on/off
VCP code: A4: Window mask control
VCP code: A5: Change the selected window
VCP code: AA: Screen Orientation
VCP code: AC: Horizontal frequency
VCP code: AE: Vertical frequency
VCP code: B0: Settings
VCP code: B2: Flat panel sub-pixel layout
VCP code: B4: Source Timing Mode
VCP code: B6: Display technology type
VCP code: B7: Monitor status
VCP code: B8: Packet count
VCP code: B9: Monitor X origin
VCP code: BA: Monitor Y origin
VCP code: BB: Header error count
VCP code: BC: Body CRC error count
VCP code: BD: Client ID
VCP code: BE: Link control
VCP code: C0: Display usage time
VCP code: C2: Display descriptor length
VCP code: C3: Transmit display descriptor
VCP code: C4: Enable display of 'display descriptor'
VCP code: C6: Application enable key
VCP code: C8: Display controller type
VCP code: C9: Display firmware level
VCP code: CA: OSD/Button Control
VCP code: CC: OSD Language
VCP code: CD: Status Indicators
VCP code: CE: Auxiliary display size
VCP code: CF: Auxiliary display data
VCP code: D0: Output select
VCP code: D2: Asset Tag
VCP code: D4: Stereo video mode
VCP code: D6: Power mode
VCP code: D7: Auxiliary power output
VCP code: DA: Scan mode
VCP code: DB: Image Mode
VCP code: DC: Display Mode
VCP code: DE: Scratch Pad
VCP code: DF: VCP Version
~~~


Command **ddcutil vcpinfo 60 --verbose*** reports everyting that **ddcutil** knows about a particular feature, including feature values: 

~~~text
$ ddcutil  vcpinfo 60 --verbose  
VCP code 60: Input Source
   Selects active video source
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: VGA-1
      0x02: VGA-2
      0x03: DVI-1
      0x04: DVI-2
      0x05: Composite video 1
      0x06: Composite video 2
      0x07: S-Video-1
      0x08: S-Video-2
      0x09: Tuner-1
      0x0a: Tuner-2
      0x0b: Tuner-3
      0x0c: Component video (YPrPb/YCrCb) 1
      0x0d: Component video (YPrPb/YCrCb) 2
      0x0e: Component video (YPrPb/YCrCb) 3
      0x0f: DisplayPort-1
      0x10: DisplayPort-2
      0x11: HDMI-1
      0x12: HDMI-2


~~~

This final example reports everyting that **ddcutil** knows about every feature definition.

~~~text
$ ddcutil vcpinfo -v
VCP code 01: Degauss
   Causes a CRT to perform a degauss cycle
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: CRT
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 02: New control value
   Indicates that a display user control (other than power) has been used to change and save (or autosave) a new value.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (complex)
   Simple NC values:
      0x01: No new control values
      0x02: One or more new control values have been saved
      0xff: No user controls are present
VCP code 03: Soft controls
   Allows display controls to be used as soft keys
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: No button active
      0x01: Button 1 active
      0x02: Button 2 active
      0x03: Button 3 active
      0x04: Button 4 active
      0x05: Button 5 active
      0x06: Button 6 active
      0x07: Button 7 active
      0xff: No user controls are present
VCP code 04: Restore factory defaults
   Restore all factory presets including brightness/contrast, geometry, color, and TV defaults.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: COLOR
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 05: Restore factory brightness/contrast defaults
   Restore factory defaults for brightness and contrast
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: COLOR
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 06: Restore factory geometry defaults
   Restore factory defaults for geometry adjustments
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: 
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 08: Restore color defaults
   Restore factory defaults for color settings.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: COLOR
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 0A: Restore factory TV defaults
   Restore factory defaults for TV functions.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: TV
   Attributes: Write Only, Non-Continuous (write-only)
VCP code 0B: Color temperature increment
   Color temperature increment used by feature 0Ch Color Temperature Request
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Only, Non-Continuous (complex)
VCP code 0C: Color temperature request
   Specifies a color temperature (degrees Kelvin)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (complex)
VCP code 0E: Clock
   Increase/decrease the sampling clock frequency.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 10: Brightness
   Increase/decrease the brightness of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 11: Flesh tone enhancement
   Select contrast enhancement algorithm respecting flesh tone region
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Non-Continuous (complex)
VCP code 12: Contrast
   Increase/decrease the contrast of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 13: Backlight control
   Increase/decrease the specified backlight control value
   MCCS versions: 2.1, 3.0
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes (v2.1): Read Write, Continuous (complex)
   Attributes (v3.0): Read Write, Continuous (complex)
   Attributes (v2.2): Deprecated, 
VCP code 14: Select color preset
   Select a specified color temperature
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Non-Continuous (complex)
   Attributes (v2.2): Read Write, Non-Continuous (complex)
   Simple NC values:
      0x01: sRGB
      0x02: Display Native
      0x03: 4000 K
      0x04: 5000 K
      0x05: 6500 K
      0x06: 7500 K
      0x07: 8200 K
      0x08: 9300 K
      0x09: 10000 K
      0x0a: 11500 K
      0x0b: User 1
      0x0c: User 2
      0x0d: User 3
VCP code 16: Video gain: Red
   Increase/decrease the luminesence of red pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 17: User color vision compensation
   Increase/decrease the degree of compensation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 18: Video gain: Green
   Increase/decrease the luminesence of green pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 1A: Video gain: Blue
   Increase/decrease the luminesence of blue pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 1C: Focus
   Increase/decrease the focus of the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 1E: Auto setup
   Perform autosetup function (H/V position, clock, clock phase, A/D converter, etc.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Auto setup not active
      0x01: Performing auto setup
      0x02: Enable continuous/periodic auto setup
VCP code 1F: Auto color setup
   Perform color autosetup function (R/G/B gain and offset, A/D setup, etc. 
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Auto setup not active
      0x01: Performing auto setup
      0x02: Enable continuous/periodic auto setup
VCP code 20: Horizontal Position (Phase)
   Increasing (decreasing) this value moves the image toward the right (left) of the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 22: Horizontal Size
   Increase/decrease the width of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 24: Horizontal Pincushion
   Increasing (decreasing) this value causes the right and left sides of the image to become more (less) convex.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 26: Horizontal Pincushion Balance
   Increasing (decreasing) this value moves the center section of the image toward the right (left) side of the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 28: Horizontal Convergence R/B
   Increasing (decreasing) this value shifts the red pixels to the right (left) and the blue pixels left (right) across the image with respect to the green pixels.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 29: Horizontal Convergence M/G
   Increasing (decreasing) this value shifts the magenta pixels to the right (left) and the green pixels left (right) across the image with respect to the magenta (sic) pixels.
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 2A: Horizontal Linearity
   Increase/decrease the density of pixels in the image center.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 2C: Horizontal Linearity Balance
   Increasing (decreasing) this value shifts the density of pixels from the left (right) side to the right (left) side of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 2E: Gray scale expansion
   Gray Scale Expansion
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Non-Continuous (complex)
VCP code 30: Vertical Position (Phase)
   Increasing (decreasing) this value moves the image toward the top (bottom) edge of the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 32: Vertical Size
   Increase/decreasing the height of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 34: Vertical Pincushion
   Increasing (decreasing) this value will cause the top and bottom edges of the image to become more (less) convex.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 36: Vertical Pincushion Balance
   Increasing (decreasing) this value will move the center section of the image toward the top (bottom) edge of the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 38: Vertical Convergence R/B
   Increasing (decreasing) this value shifts the red pixels up (down) across the image and the blue pixels down (up) across the image with respect to the green pixels.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 39: Vertical Convergence M/G
   Increasing (decreasing) this value shifts the magenta pixels up (down) across the image and the green pixels down (up) across the image with respect to the magenta (sic) pixels.
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 3A: Vertical Linearity
   Increase/decease the density of scan lines in the image center.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 3C: Vertical Linearity Balance
   Increase/decrease the density of scan lines in the image center.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 3E: Clock phase
   Increase/decrease the sampling clock phase shift
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 40: Horizontal Parallelogram
   Increasing (decreasing) this value shifts the top section of the image to the right (left) with respect to the bottom section of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 41: Vertical Parallelogram
   Increasing (decreasing) this value shifts the top section of the image to the right (left) with respect to the bottom section of the image. (sic)
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 42: Horizontal Keystone
   Increasing (decreasing) this value will increase (decrease) the ratio between the horizontal size at the top of the image and the horizontal size at the bottom of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 43: Vertical Keystone
   Increasing (decreasing) this value will increase (decrease) the ratio between the vertical size at the left of the image and the vertical size at the right of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 44: Rotation
   Increasing (decreasing) this value rotates the image (counter) clockwise around the center point of the screen.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 46: Top Corner Flare
   Increase/decrease the distance between the left and right sides at the top of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 48: Top Corner Hook
   Increasing (decreasing) this value moves the top of the image to the right (left).
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 4A: Bottom Corner Flare
   Increase/decrease the distance between the left and right sides at the bottom of the image.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 4C: Bottom Corner Hook
   Increasing (decreasing) this value moves the bottom end of the image to the right (left).
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 52: Active control
   Read id of one feature that has changed, 0x00 indicates no more
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)
VCP code 54: Performance Preservation
   Controls features aimed at preserving display performance
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (complex)
VCP code 56: Horizontal Moire
   Increase/decrease horizontal moire cancellation.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 58: Vertical Moire
   Increase/decrease vertical moire cancellation.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 59: 6 axis saturation: Red
   Increase/decrease red saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 5A: 6 axis saturation: Yellow
   Increase/decrease yellow saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 5B: 6 axis saturation: Green
   Increase/decrease green saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 5C: 6 axis saturation: Cyan
   Increase/decrease cyan saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 5D: 6 axis saturation: Blue
   Increase/decrease blue saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 5E: 6 axis saturation: Magenta
   Increase/decrease magenta saturation
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 60: Input Source
   Selects active video source
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: VGA-1
      0x02: VGA-2
      0x03: DVI-1
      0x04: DVI-2
      0x05: Composite video 1
      0x06: Composite video 2
      0x07: S-Video-1
      0x08: S-Video-2
      0x09: Tuner-1
      0x0a: Tuner-2
      0x0b: Tuner-3
      0x0c: Component video (YPrPb/YCrCb) 1
      0x0d: Component video (YPrPb/YCrCb) 2
      0x0e: Component video (YPrPb/YCrCb) 3
      0x0f: DisplayPort-1
      0x10: DisplayPort-2
      0x11: HDMI-1
      0x12: HDMI-2
VCP code 62: Audio speaker volume
   Adjusts speaker volume
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Read Write, Non-Continuous with continuous subrange
   Attributes (v2.2): Read Write, Non-Continuous with continuous subrange
VCP code 63: Speaker Select
   Selects a group of speakers
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Front L/R
      0x01: Side L/R
      0x02: Rear L/R
      0x03: Center/Subwoofer
VCP code 64: Audio: Microphone Volume
   Increase/decrease microphone gain
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes: Read Write, Continuous (normal)
VCP code 66: Ambient light sensor
   Enable/Disable ambient light sensor
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: Disabled
      0x02: Enabled
VCP code 6B: Backlight Level: White
   Increase/decrease the white backlight level
   MCCS versions: 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 6C: Video black level: Red
   Increase/decrease the black level of red pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 6D: Backlight Level: Red
   Increase/decrease the red backlight level
   MCCS versions: 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 6E: Video black level: Green
   Increase/decrease the black level of green pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 6F: Backlight Level: Green
   Increase/decrease the green backlight level
   MCCS versions: 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 70: Video black level: Blue
   Increase/decrease the black level of blue pixels
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 71: Backlight Level: Blue
   Increase/decrease the blue backlight level
   MCCS versions: 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 72: Gamma
   Select relative or absolute gamma
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Non-Continuous (complex)
VCP code 73: LUT Size
   Provides the size (number of entries and number of bits/entry) for the Red, Green, and Blue LUT in the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: LUT
   Attributes: Read Only, Table (normal)
VCP code 74: Single point LUT operation
   Writes a single point within the display's LUT, reads a single point from the LUT
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: LUT
   Attributes: Read Write, Table (normal)
VCP code 75: Block LUT operation
   Load (read) multiple values into (from) the display's LUT
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: LUT
   Attributes: Read Write, Table (normal)
VCP code 76: Remote Procedure Call
   Initiates a routine resident in the display
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: LUT
   Attributes: Write Only, Table (write-only)
VCP code 78: Display Identification Operation
   Causes a selected 128 byte block of Display Identification Data (EDID or Display ID) to be read
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.1): Read Only, Table (normal)
   Attributes (v3.0): Read Only, Table (normal)
   Attributes (v2.2): Read Only, Table (normal)
VCP code 7A: Adjust Focal Plane
   Increase/decrease the distance to the focal plane of the image
   MCCS versions: 2.0, 2.1
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Deprecated, 
   Attributes (v2.2): Deprecated, 
VCP code 7C: Adjust Zoom
   Increase/decrease the distance to the zoom function of the projection lens (optics)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Continuous (normal)
VCP code 7E: Trapezoid
   Increase/decrease the trapezoid distortion in the image
   MCCS versions: 2.0, 2.1
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Deprecated, 
   Attributes (v2.2): Deprecated, 
VCP code 80: Keystone
   Increase/decrease the keystone distortion in the image.
   MCCS versions: 2.0
   MCCS specification groups: Geometry
   ddcutil feature subsets: CRT
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Deprecated, 
   Attributes (v3.0): Deprecated, 
   Attributes (v2.2): Deprecated, 
VCP code 82: Horizontal Mirror (Flip)
   Flip picture horizontally
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Geometry
   ddcutil feature subsets: 
   Attributes (v2.0): Write Only, Non-Continuous (write-only)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Non-Continuous (simple)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Normal mode
      0x01: Mirrored horizontally mode
VCP code 84: Vertical Mirror (Flip)
   Flip picture vertically
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Geometry
   ddcutil feature subsets: 
   Attributes (v2.0): Write Only, Non-Continuous (write-only)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Non-Continuous (simple)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Normal mode
      0x01: Mirrored vertically mode
VCP code 86: Display Scaling
   Control the scaling (input vs output) of the display
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: No scaling
      0x02: Max image, no aspect ration distortion
      0x03: Max vertical image, no aspect ratio distortion
      0x04: Max horizontal image, no aspect ratio distortion
      0x05: Max vertical image with aspect ratio distortion
      0x06: Max horizontal image with aspect ratio distortion
      0x07: Linear expansion (compression) on horizontal axis
      0x08: Linear expansion (compression) on h and v axes
      0x09: Squeeze mode
      0x0a: Non-linear expansion
VCP code 87: Sharpness
   Selects one of a range of algorithms. Increasing (decreasing) the value must increase (decrease) the edge sharpness of image features.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Read Write, Continuous (normal)
   Attributes (v2.2): Read Write, Continuous (normal)
   Simple NC values:
      0x01: Filter function 1
      0x02: Filter function 2
      0x03: Filter function 3
      0x04: Filter function 4
VCP code 88: Velocity Scan Modulation
   Increase (decrease) the velocity modulation of the horizontal scan as a function of the change in luminescence level
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: CRT
   Attributes: Read Write, Continuous (normal)
VCP code 8A: Color Saturation
   Increase/decrease the amplitude of the color difference components of the video signal
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR, TV
   Attributes: Read Write, Continuous (normal)
VCP code 8B: TV Channel Up/Down
   Increment (1) or decrement (2) television channel
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: TV
   Attributes: Write Only, Non-Continuous (write-only)
   Simple NC values:
      0x01: Increment channel
      0x02: Decrement channel
VCP code 8C: TV Sharpness
   Increase/decrease the amplitude of the high frequency components  of the video signal
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: TV
   Attributes: Read Write, Continuous (normal)
VCP code 8D: Audio mute/Screen blank
   Mute/unmute audio, and (v2.2) screen blank
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: TV, AUDIO
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Non-Continuous (simple)
   Attributes (v2.2): Read Write, Non-Continuous (complex)
   Simple NC values:
      0x01: Mute the audio
      0x02: Unmute the audio
VCP code 8E: TV Contrast
   Increase/decrease the ratio between blacks and whites in the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: TV
   Attributes: Read Write, Continuous (normal)
VCP code 8F: Audio Treble
   Emphasize/de-emphasize high frequency audio
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Read Write, Non-Continuous with continuous subrange
   Attributes (v2.2): Read Write, Non-Continuous with continuous subrange
VCP code 90: Hue
   Increase/decrease the wavelength of the color component of the video signal. AKA tint.  Applies to currently active interface
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: COLOR, TV
   Attributes: Read Write, Continuous (normal)
VCP code 91: Audio Bass
   Emphasize/de-emphasize low frequency audio
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Read Write, Non-Continuous with continuous subrange
   Attributes (v2.2): Read Write, Non-Continuous with continuous subrange
VCP code 92: TV Black level/Luminesence
   Increase/decrease the black level of the video
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: TV
   Attributes: Read Write, Continuous (normal)
VCP code 93: Audio Balance L/R
   Controls left/right audio balance
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: AUDIO
   Attributes (v2.0): Read Write, Continuous (normal)
   Attributes (v2.1): Read Write, Continuous (normal)
   Attributes (v3.0): Read Write, Non-Continuous with continuous subrange
   Attributes (v2.2): Read Write, Non-Continuous with continuous subrange
VCP code 94: Audio Processor Mode
   Select audio mode
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Audio
   ddcutil feature subsets: TV, AUDIO
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Speaker off/Audio not supported
      0x01: Mono
      0x02: Stereo
      0x03: Stereo expanded
      0x11: SRS 2.0
      0x12: SRS 2.1
      0x13: SRS 3.1
      0x14: SRS 4.1
      0x15: SRS 5.1
      0x16: SRS 6.1
      0x17: SRS 7.1
      0x21: Dolby 2.0
      0x22: Dolby 2.1
      0x23: Dolby 3.1
      0x24: Dolby 4.1
      0x25: Dolby 5.1
      0x26: Dolby 6.1
      0x27: Dolby 7.1
      0x31: THX 2.0
      0x32: THX 2.1
      0x33: THX 3.1
      0x34: THX 4.1
      0x35: THX 5.1
      0x36: THX 6.1
      0x37: THX 7.1
VCP code 95: Window Position(TL_X)
   Top left X pixel of an area of the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Continuous (normal)
VCP code 96: Window Position(TL_Y)
   Top left Y pixel of an area of the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Continuous (normal)
VCP code 97: Window Position(BR_X)
   Bottom right X pixel of an area of the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Continuous (normal)
VCP code 98: Window Position(BR_Y)
   Bottom right Y pixel of an area of the image
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Geometry, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Continuous (normal)
VCP code 99: Window control on/off
   Enables the brightness and color within a window to be different from the desktop.
   MCCS versions: 2.0, 2.1
   MCCS specification groups: Window
   ddcutil feature subsets: WINDOW
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Deprecated, 
   Attributes (v2.2): Deprecated, 
   Simple NC values:
      0x00: No effect
      0x01: Off
      0x02: On
VCP code 9A: Window background
   Changes the contrast ratio between the area of the window and the rest of the desktop
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Continuous (normal)
VCP code 9B: 6 axis hue control: Red
   Decrease shifts toward magenta, increase shifts toward yellow
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 9C: 6 axis hue control: Yellow
   Decrease shifts toward green, increase shifts toward red
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 9D: 6 axis hue control: Green
   Decrease shifts toward yellow, increase shifts toward cyan
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 9E: 6 axis hue control: Cyan
   Decrease shifts toward green, increase shifts toward blue
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code 9F: 6 axis hue control: Blue
   Decrease shifts toward cyan, increase shifts toward magenta
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code A0: 6 axis hue control: Magenta
   Decrease shifts toward blue, 127 no effect, increase shifts toward red
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: PROFILE, COLOR
   Attributes: Read Write, Continuous (normal)
VCP code A2: Auto setup on/off
   Turn on/off an auto setup function
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: 
   Attributes: Write Only, Non-Continuous (write-only)
   Simple NC values:
      0x01: Off
      0x02: On
VCP code A4: Window mask control
   Turn selected window operation on/off, window mask
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: WINDOW
   Attributes (v2.0): Read Write, Non-Continuous (complex)
   Attributes (v2.1): Read Write, Non-Continuous (complex)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Table (normal)
VCP code A5: Change the selected window
   Change selected window (as defined by 95h..98h)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Window
   ddcutil feature subsets: WINDOW
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Full display image area selected except active windows
      0x01: Window 1 selected
      0x02: Window 2 selected
      0x03: Window 3 selected
      0x04: Window 4 selected
      0x05: Window 5 selected
      0x06: Window 6 selected
      0x07: Window 7 selected
VCP code AA: Screen Orientation
   Indicates screen orientation
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Geometry
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (simple)
   Simple NC values:
      0x01: 0 degrees
      0x02: 90 degrees
      0x03: 180 degrees
      0x04: 270 degrees
      0xff: Display cannot supply orientation
VCP code AC: Horizontal frequency
   Horizontal sync signal frequency as determined by the display
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Continuous (complex)
VCP code AE: Vertical frequency
   Vertical sync signal frequency as determined by the display, in .01 hz
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Continuous (complex)
VCP code B0: Settings
   Store/restore the user saved values for the current mode.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Preset
   ddcutil feature subsets: 
   Attributes: Write Only, Non-Continuous (write-only)
   Simple NC values:
      0x01: Store current settings in the monitor
      0x02: Restore factory defaults for current mode
VCP code B2: Flat panel sub-pixel layout
   LCD sub-pixel structure
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (simple)
   Simple NC values:
      0x00: Sub-pixel layout not defined
      0x01: Red/Green/Blue vertical stripe
      0x02: Red/Green/Blue horizontal stripe
      0x03: Blue/Green/Red vertical stripe
      0x04: Blue/Green/Red horizontal stripe
      0x05: Quad pixel, red at top left
      0x06: Quad pixel, red at bottom left
      0x07: Delta (triad)
      0x08: Mosaic
VCP code B4: Source Timing Mode
   Indicates timing mode being sent by host
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Control
   ddcutil feature subsets: 
   Attributes (v2.1): Read Write, Non-Continuous (complex)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Table (normal)
VCP code B6: Display technology type
   Indicates the base technology type
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (simple)
   Simple NC values:
      0x01: CRT (shadow mask)
      0x02: CRT (aperture grill)
      0x03: LCD (active matrix)
      0x04: LCos
      0x05: Plasma
      0x06: OLED
      0x07: EL
      0x08: MEM
VCP code B7: Monitor status
   Video mode and status of a DPVL capable monitor
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Only, Non-Continuous (complex)
VCP code B8: Packet count
   Counter for DPVL packets received
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code B9: Monitor X origin
   X origin of the monitor in the vertical screen
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code BA: Monitor Y origin
   Y origin of the monitor in the vertical screen
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code BB: Header error count
   Error counter for the DPVL header
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code BC: Body CRC error count
   CRC error counter for the DPVL body
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code BD: Client ID
   Assigned identification number for the monitor
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Continuous (complex)
VCP code BE: Link control
   Indicates status of the DVI link
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: DPVL
   ddcutil feature subsets: DPVL
   Attributes: Read Write, Non-Continuous (complex)
VCP code C0: Display usage time
   Active power on time in hours
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Continuous (complex)
VCP code C2: Display descriptor length
   Length in bytes of non-volatile storage in the display available for writing a display descriptor, max 256
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Continuous (normal)
VCP code C3: Transmit display descriptor
   Reads (writes) a display descriptor from (to) non-volatile storage in the display.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Table (normal)
VCP code C4: Enable display of 'display descriptor'
   If enabled, the display descriptor shall be displayed when no video is being received.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (complex)
VCP code C6: Application enable key
   A 2 byte value used to allow an application to only operate with known products.
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)
VCP code C8: Display controller type
   Mfg id of controller and 2 byte manufacturer-specific controller type
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Control, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)
   Simple NC values:
      0x01: Conexant
      0x02: Genesis
      0x03: Macronix
      0x04: IDT
      0x05: Mstar
      0x06: Myson
      0x07: Phillips
      0x08: PixelWorks
      0x09: RealTek
      0x0a: Sage
      0x0b: Silicon Image
      0x0c: SmartASIC
      0x0d: STMicroelectronics
      0x0e: Topro
      0x0f: Trumpion
      0x10: Welltrend
      0x11: Samsung
      0x12: Novatek
      0x13: STK
      0x14: Silicon Optics
      0x15: Texas Instruments
      0x16: Analogix
      0x17: Quantum Data
      0x18: NXP Semiconductors
      0x19: Chrontel
      0x1a: Parade Technologies
      0x1b: THine Electronics
      0x1c: Trident
      0x1d: Micros
      0xff: Not defined - a manufacturer designed controller
VCP code C9: Display firmware level
   2 byte firmware level
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Control, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)
VCP code CA: OSD/Button Control
   Sets and indicates the current operational state of OSD (and buttons in v2.2)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Control, Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Non-Continuous (simple)
   Attributes (v2.2): Read Write, Non-Continuous (complex)
   Simple NC values:
      0x01: OSD Disabled
      0x02: OSD Enabled
      0xff: Display cannot supply this information
VCP code CC: OSD Language
   On Screen Display language
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Control, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Reserved value, must be ignored
      0x01: Chinese (traditional, Hantai)
      0x02: English
      0x03: French
      0x04: German
      0x05: Italian
      0x06: Japanese
      0x07: Korean
      0x08: Portuguese (Portugal)
      0x09: Russian
      0x0a: Spanish
      0x0b: Swedish
      0x0c: Turkish
      0x0d: Chinese (simplified / Kantai)
      0x0e: Portuguese (Brazil)
      0x0f: Arabic
      0x10: Bulgarian 
      0x11: Croatian
      0x12: Czech
      0x13: Danish
      0x14: Dutch
      0x15: Estonian
      0x16: Finnish
      0x17: Greek
      0x18: Hebrew
      0x19: Hindi
      0x1a: Hungarian
      0x1b: Latvian
      0x1c: Lithuanian
      0x1d: Norwegian 
      0x1e: Polish
      0x1f: Romanian 
      0x20: Serbian
      0x21: Slovak
      0x22: Slovenian
      0x23: Thai
      0x24: Ukranian
      0x25: Vietnamese
VCP code CD: Status Indicators
   Control up to 16 LED (or similar) indicators to indicate system status
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (complex)
VCP code CE: Auxiliary display size
   Rows and characters/row of auxiliary display
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)
VCP code CF: Auxiliary display data
   Sets contents of auxiliary display device
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Write Only, Table (write-only)
VCP code D0: Output select
   Selects the active output
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Read Write, Non-Continuous (simple)
   Attributes (v2.1): Read Write, Non-Continuous (simple)
   Attributes (v3.0): Read Write, Table (normal)
   Attributes (v2.2): Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: Analog video (R/G/B) 1
      0x02: Analog video (R/G/B) 2
      0x03: Digital video (TDMS) 1
      0x04: Digital video (TDMS) 22
      0x05: Composite video 1
      0x06: Composite video 2
      0x07: S-Video-1
      0x08: S-Video-2
      0x09: Tuner-1
      0x0a: Tuner-2
      0x0b: Tuner-3
      0x0c: Component video (YPrPb/YCrCb) 1
      0x0d: Component video (YPrPb/YCrCb) 2
      0x0e: Component video (YPrPb/YCrCb) 3
      0x0f: DisplayPort-1
      0x10: DisplayPort-2
      0x11: HDMI-1
      0x12: HDMI-2
VCP code D2: Asset Tag
   Read an Asset Tag to/from the display
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Table (normal)
VCP code D4: Stereo video mode
   Stereo video mode
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (complex)
VCP code D6: Power mode
   DPM and DPMS status
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Control, Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: DPM: On,  DPMS: Off
      0x02: DPM: Off, DPMS: Standby
      0x03: DPM: Off, DPMS: Suspend
      0x04: DPM: Off, DPMS: Off
      0x05: Write only value to turn off display
VCP code D7: Auxiliary power output
   Controls an auxiliary power output from a display to a host device
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x01: Disable auxiliary power
      0x02: Enable Auxiliary power
VCP code DA: Scan mode
   Controls scan characteristics (aka format)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image, Geometry
   ddcutil feature subsets: CRT
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Normal operation
      0x01: Underscan
      0x02: Overscan
      0x03: Widescreen
VCP code DB: Image Mode
   Controls aspects of the displayed image (TV applications)
   MCCS versions: 2.1, 3.0, 2.2
   MCCS specification groups: Control
   ddcutil feature subsets: TV
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: No effect
      0x01: Full mode
      0x02: Zoom mode
      0x03: Squeeze mode
      0x04: Variable
VCP code DC: Display Mode
   Type of application used on display
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Image
   ddcutil feature subsets: COLOR
   Attributes: Read Write, Non-Continuous (simple)
   Simple NC values:
      0x00: Standard/Default mode
      0x01: Productivity
      0x02: Mixed
      0x03: Movie
      0x04: User defined
      0x05: Games
      0x06: Sports
      0x07: Professional (all signal processing disabled)
      0x08: Standard/Default mode with intermediate power consumption
      0x09: Standard/Default mode with low power consumption
      0x0a: Demonstration
      0xf0: Dynamic contrast
VCP code DE: Scratch Pad
   Operation mode (2.0) or scratch pad (3.0/2.2)
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes (v2.0): Write Only, Non-Continuous (write-only)
   Attributes (v2.1): Read Write, Non-Continuous (complex)
   Attributes (v3.0): Read Write, Non-Continuous (complex)
   Attributes (v2.2): Read Write, Non-Continuous (complex)
VCP code DF: VCP Version
   MCCS version
   MCCS versions: 2.0, 2.1, 3.0, 2.2
   MCCS specification groups: Miscellaneous
   ddcutil feature subsets: 
   Attributes: Read Only, Non-Continuous (complex)


~~~

