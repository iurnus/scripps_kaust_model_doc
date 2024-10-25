###########
Install WPS
###########

Install WPS on Shaheen or COMET
===============================

Download WPS::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WPS/archive/refs/tags/v4.5.zip
  unzip v4.5.zip
  mv WPS-4.5 WPSV45
  cd WPSV45

Update the configurations. First open ``configure``, update line 154 to add the WRF directory::
  
  standard_wrf_dirs="WRF WRF-4.0.3 WRF-4.0.2 WRF-4.0.1 WRF-4.0 WRFV3 WRFV451_AO"

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

I select "1" for kala, "17" for EXPANSE, and "37" for Shaheen.

Install
-------

Run the compile command for the real applications::

    ./compile &> log.compile &

If the code is compiled, ``geogrid.exe``, ``metgrid.exe`` and ``ungrib.exe`` can be seen in the current folder.
