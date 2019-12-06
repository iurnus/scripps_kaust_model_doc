############################
Limitations and known issues
############################

Limitations
===========

The limitations of the code are:

- One can use the unit tests to check if ESMF8.0.0 is successfully built. The coupled code can be
  successfully compiled even a few unit tests failed. We don't know which specific tests must pass
  for the coupled code.
- MITgcm is quite flexible and the current coupler cannot cover every situation in MITgcm. The
  developers are still trying to make the code to cover all the modeling conditions in MITgcm.
- The WRF implements are based on WRF I/O streams. The implementations may change the I/O streams of
  WRF (io_esmf) and disable the modeling of some cases in the WRF tutorial.

Known issues
============

There are currently no major issues with the code. If the user have any issues with the code, please
contact: Rui Sun (rus043@ucsd.edu) or Aneesh Subramanian (aneeshcs@gmail.com)
