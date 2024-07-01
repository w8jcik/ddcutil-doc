## Notes on Driver amdgpu</a name="amdgpu"></a>

Based on [Linux GPU driver documentation for amdgpu](https://www.kernel.org/doc/html/v4.20/gpu/amdgpu.html)

### Kernel option amdgpu.dc (Display Core)

Disable/Enable Display Core driver for debugging (1 = enable, 0 = disable). The default is -1 (automatic for each asic).

amdgpu.dc value | Meaning
----------------|----------
0               |&nbsp; disable Display Core
1               |&nbsp; enable  Display Core
default = -1    |nbsp;enable/disable based on asic 

<br>
TODO: insert list of GPUs supporting DC

## Kernal option amdgpu.hw_i2c

To enable hw i2c engine. Only affects non-DC display handling. The default is 0 (Disabled).

amdgpu.hw_i2c (applies only if Display Core disabled)

 amdgpu.hw_i2c value  | Meaning
----------------------|----------
0 (default)           |&nbsp;disable
1                     |&nbsp;enable

<br>

Tested on RX580 (Ellesmere), Kernel 5.13

Name in /sys/bus/i2c/devices/i2c-N/name 

Case 1: Display Core disabled and hw_i2c=0

Connector | Name
----------|----------
DVI, HDMI |&nbsp; AMDGPU i2c bit bus 0xNN
DP        |&nbsp; CardM-DP-N, e.g. Card0-DP-1

<br>

Case 2: Display Core disabled and hw_i2c=1

Connector | Name
----------|----------
DVI, HDMI |&nbsp;AMDGPU i2c hw bus 0xNN
DP        |&nbsp;CardM-DP-N, e.g. Card0-DP-1

<br>
I2C communication fails on DVI and HDMI
<br>

Case 3: Display Core enabled

<br>

Connector | Name
----------|----------
DVI, HDMI |&nbsp;AMDGPU DM i2c hw bus 0x9N
DP        |&nbsp;AMDGPU DM aux hw bus N

<br>
<p>
Where is /sys/bus/i2c/devices/i2c-name set?
<p>
Message Format           | Message Source
-------------------------|----------------
AMDGPU DM aux hw bus N   |&nbsp;drivers/gpu/drm/amd/display/amdgpu_dm/amdgpu_dm_mst_types.c
AMDGPU i2c hw bus 0xNN   |&nbsp;drivers/gpu/drm/amd/amdgpu/amdgpu_i2c.c
AMDGPU i2c bit bus 0xNN  |&nbsp;drivers/gpu/drm/amd/amdgpu/amdgpu_i2c.c

<p>
If Display Core is enabled, a line containining either  "Display Core initialized" or "Display Core failed to initialize" is found in the system log.

Message source: drivers/gpu/drm/amd/display/amdgpu_dm/amdgpu_dm.c, drivers/gpu/drm/amd/display/dc/core/dc.c

To check: 
~~~
# egrep -e "Kernel command line|Display Core"  /var/log/syslog
~~~

