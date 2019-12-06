###########
Install WPS
###########

How to get WPS code
===================

The WPS code is available at:

http://www2.mmm.ucar.edu/wrf/users/download/get_source.html

Register an account, then download the WRF-ARW code, WPS code, WRFDA/WRFPLUS code, and WRF-Chemistry
code. 

Install WPS using the default compiler
======================================

Setup JASPER
------------
First, we need to install jasper, libpng and zlib. Then, put their path to the bashrc (or you can do
it in the command line)::

    export JASPERINC=/project_shared/Libraries/jasper-1.900.1_gnu_fortran_4.8.5-11/include/
    export JASPERINC="${JASPERINC} -I/project_shared/Libraries/libpng-1.6.30_gnu_fortran_4.8.5-11/include/"
    export JASPERINC="${JASPERINC} -I/project_shared/Libraries/zlib-1.2.11_gnu_fortran_4.8.5-11/include/"
    export JASPERLIB=/project_shared/Libraries/jasper-1.900.1_gnu_fortran_4.8.5-11/lib/
    export JASPERLIB="${JASPERLIB} -L/project_shared/Libraries/libpng-1.6.30_gnu_fortran_4.8.5-11/lib/"
    export JASPERLIB="${JASPERLIB} -L/project_shared/Libraries/zlib-1.2.11_gnu_fortran_4.8.5-11/lib/"


Configuring
-----------

Run the configuration command::
  
    ./configure

The screen would say::

    Please select from among the following supported platforms.

        1.  Linux x86_64, gfortran    (serial)
        2.  Linux x86_64, gfortran    (serial_NO_GRIB2)
        3.  Linux x86_64, gfortran    (dmpar)
        4.  Linux x86_64, gfortran    (dmpar_NO_GRIB2)
        5.  Linux x86_64, PGI compiler   (serial)
        6.  Linux x86_64, PGI compiler   (serial_NO_GRIB2)
        7.  Linux x86_64, PGI compiler   (dmpar)
        8.  Linux x86_64, PGI compiler   (dmpar_NO_GRIB2)
    (and some more options)
    Enter selection [1-51] :

Select "5" for the supported platform.

Check file "configure.wps", make sure that WRF has been installed in the correct path::

    WRF_DIR = ../WRFV412

Install
-------

Run the compile command for the real applications::

    ./compile &> log.compile &

If the code is compiled, *geogrid.exe*, *metgrid.exe* and *ungrib.exe* can be seen in the folder
**WPS_DIR**. Here, **WPS_DIR** is the directory of the WPS software that is installed.

Test the installation
---------------------

Testing the WPS pre-processing code is detailed in :ref:`pre-precessing using WPS<pre_wps>`.

