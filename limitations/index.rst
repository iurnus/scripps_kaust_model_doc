############################
Limitations and Known Issues
############################

Limitations
===========

The limitations of the code are:

- One can use the unit tests to check if ESMF 8.6.0 is successfully built. The coupled code can be
  successfully compiled even a few unit tests failed. We don't know which specific tests must pass
  for the coupled code.
- MITgcm is flexible and the coupler cannot cover every situation in MITgcm. The
  developers are still trying to make the code to cover all the modeling conditions in MITgcm.
- The WRF implementations are based on WRF I/O streams. The implementations may change the I/O streams of
  WRF (io_esmf) and disable the modeling of some cases in the WRF tutorial.
- Because the WRF implementations are based on WRF I/O, we haven't tested it for nested grid.
- The start/stop time of MITgcm, WRF, and ESMF are set up separately. If they are not consistent,
  there might be errors reported. For example, if the stop time in MITgcm is earlier than ESMF, the
  diagnostic package will first report an error and stops the coupled run. 
- Because we do not have PGI compiler license anymore, we haven't tested it for the new developments. 
  Please check version 2.0 or 1.2 for help with PGI compilers.
  

Known issues
============

There are currently no major issues with the code. If the user have any issues with the code, please
contact: Rui Sun (rus043@ucsd.edu) or Aneesh Subramanian (aneeshcs@gmail.com)

When debuging the code, please see here:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   debug SKRIPS <debug>
