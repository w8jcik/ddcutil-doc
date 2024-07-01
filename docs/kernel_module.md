## Kernel Module Configuration

Kernel module i2c-dev must be loaded.
On some distributions, it is built into the Linux kernel.  If not, it must be loaded explicitly. 

To see if i2c-dev is built into the kernel, issue the following command: 
~~~
grep i2c-dev.ko  /lib/modules/`uname -r`/modules.builtin
~~~

If i2c-dev is not built in, add the line "i2c_dev" to /etc/modules or a file containing the single line:
~~~
i2c_dev
~~~
to directory /etc/modules-load.d

<!--
(TODO: Do instructions need to take into account SysV init vs systemd?)
-->
From the freedesktop.org systemd doc: 

>systemd-modules-load.service(8) reads files from the [following]directories
>which contain kernel modules to load during boot in a static list. 

>/etc/modules-load.d/*.conf  
>/run/modules-load.d/*.conf  
>/usr/lib/modules-load.d/*.conf  

>Each configuration file is named in the style of /etc/modules-load.d/program.conf. 

