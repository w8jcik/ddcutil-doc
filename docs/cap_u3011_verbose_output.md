## ddcutil capabilities

Command **ddcutil capabilities --brief** shows the capabilities string reported by the monitor that ddcutil has identified as display number 4:

~~~text
$ ddcutil capabilities --brief --display 4
Unparsed capabilities string: (prot(monitor)type(lcd)model(U3011)cmds(01 02 03 07 0C E3 F3)vcp(02 04 05 06 08 10 12 14(01 05 08 0B 0C) 16 18 1A 52 60(01 03 04 0C 0F 11 12) AC AE B2 B6 C6 C8 C9 D6(01 04 05) DC(00 02 03 04 05) DF FD)mccs_ver(2.1)mswhql(1))
~~~

WIth the ***--verbose*** option, **capaibilities** includes details for each feature, as specified in the Monitor Control Command Set and modified by a feature definition file:

~~~text
$ src/ddcutil --dis 4 cap --verbose
Processed feature definition file: /home/rock/.local/share/ddcutil/DEL-DELL_U3011-16485.mccs
Unparsed capabilities string: (prot(monitor)type(lcd)model(U3011)cmds(01 02 03 07 0C E3 F3)vcp(02 04 05 06 08 10 12 14(01 05 08 0B 0C) 16 18 1A 52 60(01 03 04 0C 0F 11 12) AC AE B2 B6 C6 C8 C9 D6(01 0
4 05) DC(00 02 03 04 05) DF FD)mccs_ver(2.1)mswhql(1))
Model: U3011
MCCS version: 2.1
Commands:
   Op Code: 01 (VCP Request)
   Op Code: 02 (VCP Response)
   Op Code: 03 (VCP Set)
   Op Code: 07 (Timing Request)
   Op Code: 0C (Save Settings)
   Op Code: E3 (Capabilities Reply)
   Op Code: F3 (Capabilities Request)
VCP Features:
   Feature: 02 (New control value)
   Feature: 04 (Restore factory defaults)
   Feature: 05 (Restore factory brightness/contrast defaults)
   Feature: 06 (Restore factory geometry defaults)
   Feature: 08 (Restore color defaults)
   Feature: 10 (Brightness)
   Feature: 12 (Contrast)
   Feature: 14 (Select color preset)
      Values (unparsed): 01 05 08 0B 0C
      Values (  parsed):
         01: sRGB
         05: 6500 K
         08: 9300 K
         0b: User 1
         0c: User 2
   Feature: 16 (Video gain: Red)
   Feature: 18 (Video gain: Green)
   Feature: 1A (Video gain: Blue)
   Feature: 52 (Active control)
   Feature: 60 (Input Source)
      Values (unparsed): 01 03 04 0C 0F 11 12
      Values (  parsed):
         01: VGA-1
         03: DVI-1
         04: DVI-2
         0c: Component video (YPrPb/YCrCb) 1
         0f: DisplayPort-1
         11: HDMI-1
         12: HDMI-2
   Feature: AC (Horizontal frequency)
   Feature: AE (Vertical frequency)
   Feature: B2 (Flat panel sub-pixel layout)
   Feature: B6 (Display technology type)
   Feature: C6 (Application enable key)
   Feature: C8 (Display controller type)
rock@banner:/shared/playproj/i2c$ 
~~~