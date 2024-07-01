## Techical Notes

A collection of notes related to development and debugging.

To build with debugging symbols in libddcutil: **./configure CFLAGS=-g**

[Enable I2C kernel tracing](https://michlstechblog.info/blog/linux-enable-i2c-kernel-tracing/)

From the i2c-dev mailing list: 

I have spent quite a while searching in places such as stackoverflow
and in the docmentation at linux.kernel.org but I haven't found a very
conclusive answer (no to me anyway).

I have a single I2C bus connected to a single board computer
(Beaglebone Black) running Debian 11, Kernel: 5.10.168.

Is it safe to allow multiple processes to read and write various
devices on the I2C bus?  This may be either different processes
accessing the same device or different processes accessing different
devices on the I2C bus.

I.e. is there a mutex (or equivalent code) in the Linux Kernel I2C
drivers that guarantees completion of one process's I2C transaction
before another one starts?

The information I have managed to find suggests that there is such a
mutex and that I don't need to make sure my processes don't try to
read/write the I2C bus simultaneously but I can't find a definitive
statement to this effect.

So, I'm sorry to ask such a 'user' question here but I would really
like a definite answer please.

-- 
Chris Green



Hi Chris,

On Sun, Nov 05, 2023 at 12:57:31PM +0000, Chris Green wrote:
> Is it safe to allow multiple processes to read and write various
> devices on the I2C bus?  This may be either different processes
> accessing the same device or different processes accessing different
> devices on the I2C bus.

Yes, it's safe. Different processes and different in kernel users like
drivers can access the I2C bus at the same time. The I2C transfers will
not interfere with each other. They are serialized by the kernel and
execute one after another.

> I.e. is there a mutex (or equivalent code) in the Linux Kernel I2C
> drivers that guarantees completion of one process's I2C transaction
> before another one starts?

Yes, exactly. The mutex is called 'bus_lock' and can be found here:

    https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/include/linux/i2c.h?h=v6.6&id=ffc253263a1375a65fa6c9f62a893e9767fbebfa#n727

And here the adapter/bus is locked before a transfer starts:

    https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/i2c/i2c-core-base.c?h=v6.6&id=ffc253263a1375a65fa6c9f62a893e9767fbebfa#n2279

> The information I have managed to find suggests that there is such a
> mutex and that I don't need to make sure my processes don't try to
> read/write the I2C bus simultaneously but I can't find a definitive
> statement to this effect.

So as the I2C bus is concerned, everything is safe. The I2C read/writes
are serialized. But logically there maybe issues. E.g. if two processes
or driver writes to the same I2C register of the same device at the same
time. You will not know which transfer was the last one. So you don't
know which write has won.

So you can have race conditions on a logical level.

Kind regards,
Stefan



n. code cited is in include/linux/i2c.h, drivers/i2c/i2c-core-base.c