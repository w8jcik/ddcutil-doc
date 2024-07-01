## Shared Library Changes for Release 2.1.3

Shared library libddcutil is backwardly compatible with the one in 
ddcutil 2.0.0. The SONAME is unchanged as libddcutil.so.5. The released library
file is libddcutil.so.5.1.2. 

There is one change of significance.  Release 2.1.3 fixes a bug in **libddcutil** that caused API calls to fail with status code
DDCRC_ARG (invalid argument) when passed an otherwise valid display reference for a display that doesn't support DDC/CI.
In particular, this caused **ddcui** to crash when querying information about such a display.
