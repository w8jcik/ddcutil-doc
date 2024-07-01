## Notes for Linux Distribution Maintainers

As of **ddcutil** release 2.0.0, those who maintain **ddcutil** packages for Linux distributions need 
to consider that libddcutil.5 in release 2.0.0 is not backwardly compatible with libddcutil.4
from earlier releases.  Therefore, at least for some time, both versions of the shared library should
be packaged, and application packages should indicate which is required.

The details of how packaging is done varies from one distribution to another.
As of **ddcutil** source 1.4.5, the build system has one
new option that may, depending on distribution, help in this regard.
If **configure** option ***--enable-install-lib-only*** is specified, only the shared library 
is built and installed.  Command line program **ddcutil** and the files that go in 
a development package (e.g. C header files) are not.


