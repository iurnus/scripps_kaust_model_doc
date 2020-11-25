.. _install_wrf_ring:

###########################
Install WRF on ring/shaheen
###########################


Configuring and Installing the code
-----------------------------------

The configurations are prepared for ring at UCSD and Shaheen-II. They are saved in
./installOption_WRF (current working directory: $HOME/scripps_kaust_model-1.1/)::

   ll installOption_WRF/*.sh

You'll find the following installers for WRF4.1.3::

   installOption_WRF/installWRF413_ao_ring.sh
   installOption_WRF/installWRF413_ao_shaheen.sh

To install WRF on ring at UCSD, we can run the installer (skip step 8, 9, 10, and 11)::
  
    # on ring
    cp installOption_WRF/installWRF413_ao_ring.sh .
    ./installWRF413_ao_ring.sh

Similarly for Shaheen-II, we can run the installer (skip step 8, 9, 10, and 11)::

    # on Shaheen-II
    cp installOption_WRF/installWRF413_ao_shaheen.sh .
    ./installWRF413_ao_shaheen.sh

If the code is compiled, *real.exe*, *wrf.exe*, *ndown.exe*, and *tc.exe* can be seen in the folder
WRFV413_AO/main. Here, *413* means WRF413, *AO* means coupled ocean--atmosphere code. To test the
installation of the coupled WRF--ESMF code, a simple test case is available in the :ref:`WRF test,
real cases<test_wrf_real>`.

Other guidance to compile WRF
-----------------------------

There is another guidance to compile WRF available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php

