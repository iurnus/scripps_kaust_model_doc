.. _install_wrf:

###########
Install WRF
###########

How to get WRF code
===================

The WRF code is available at:

http://www2.mmm.ucar.edu/wrf/users/download/get_source.html

Register an account, then download the WRF-ARW code, WPS code, WRFDA/WRFPLUS code, and WRF-Chemistry
code. 


Install WRF coupled with ESMF, using NetCDF4 and OpenMPI
========================================================

Setup NetCDF4
-------------
First, we need to set NetCDF4 in the bashrc or in the command line (These options only work for ring@ucsd.edu)::

    export NETCDF=/project_shared/Libraries/netcdf-fortran-4.4.4_pgi_fortran_17.5-0/
    export LD_LIBRARY_PATH=/project_shared/Libraries/netcdf-4.4.1.1_pgi_fortran_17.5-0/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
    export LD_LIBRARY_PATH=/project_shared/Libraries/netcdf-cxx4-4.3.0_pgi_fortran_17.5-0/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
    export LD_LIBRARY_PATH=/project_shared/Libraries/netcdf-fortran-4.4.4_pgi_fortran_17.5-0/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
    export LD_LIBRARY_PATH=/home/rus043/kaust_project/mitgcm-esmf-wrf/esmf/lib/libg/Linux.pgi.64.openmpi.default${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
    export LD_LIBRARY_PATH=/usr/lib64${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

Configuring and Installing the code
-----------------------------------

Check the WRF install options in installOption_WRF folder ::

   ll installOption_WRF/*.sh

You'll find the following installers for WRF3.6 WRF3.911::

   installOption_WRF/installWRF36_ao_ring.sh
   installOption_WRF/installWRF36_ao_tscc.sh
   installOption_WRF/installWRF36_aow_ring.sh
   installOption_WRF/installWRF36_aow_tscc.sh
   installOption_WRF/installWRF3911_ao_ring.sh
   installOption_WRF/installWRF3911_ao_tscc.sh
   installOption_WRF/installWRF3911_aow_ring.sh
   installOption_WRF/installWRF3911_aow_tscc.sh
   installOption_WRF/installWRF_esmftest.sh
   installOption_WRF/installWRF_mitbulk.sh

Run the installer::
  
    cp installOption_WRF/installWRF3911_ao_ring.sh .
    ./installWRF3911_ao_ring.sh

If we take a look inside the installer, we can see::

    1 echo "installing WRF"
    2 WRF_PWD_DIR=${PWD}
    3 cd WRFV3911_AO
    4 echo "WRF_PWD_DIR is: ${WRF_PWD_DIR}"
    5 WRF_OPTION_DIR=${WRF_PWD_DIR}/installOption_WRF/wrfAO3911Implementations_ring/
    6
    7 echo "Deleting old configure file..."
    8 rm -rf configure.wrf
    9
   10 # WRF configure=54, then nesting=1
   11 echo "choosing 54th option to compile WRF"
   12 echo "nesting option is 1 (normal)"
   13 printf '54\n1\n' | ./configure &> log.configure
   14
   15 echo "copying other files to compile ESMF--WRF"
   16 ln -sf ${WRF_OPTION_DIR}/Makefile.main main/Makefile
   17 ln -sf ${WRF_OPTION_DIR}/wrf_test_ESMF.F main/
   18 ln -sf ${WRF_OPTION_DIR}/Makefile.wrf Makefile
   19 ln -sf ${WRF_OPTION_DIR}/makefile.io_netcdf external/io_netcdf/makefile
   20 ln -sf ${WRF_OPTION_DIR}/makefile.io_esmf external/io_esmf/makefile
   21 ln -sf ${WRF_OPTION_DIR}/module_domain.F frame/
   22 ln -sf ${WRF_OPTION_DIR}/ext_esmf_write_field.F90 external/io_esmf/
   23 ln -sf ${WRF_OPTION_DIR}/ext_esmf_read_field.F90 external/io_esmf/
   24 ln -sf ${WRF_OPTION_DIR}/ext_esmf_open_for_read.F90 external/io_esmf/
   25 ln -sf ${WRF_OPTION_DIR}/ext_esmf_open_for_write.F90 external/io_esmf/
   26 ln -sf ${WRF_OPTION_DIR}/io_esmf.F90 external/io_esmf/
   27 ln -sf ${WRF_OPTION_DIR}/module_esmf_extensions.F90 external/io_esmf/
   28 ln -sf ${WRF_OPTION_DIR}/module_diag_rasm.F phys/
   29 ln -sf ${WRF_OPTION_DIR}/Registry.EM Registry/Registry.EM
   30 ln -sf ${WRF_OPTION_DIR}/Registry.EM_COMMON_direct Registry/Registry.EM_COMMON
   31 ln -sf ${WRF_OPTION_DIR}/configure.wrf configure.wrf
   32
   33 echo "compiling WRFv3.9.1.1"
   34 ./compile em_real &> log.em_real1
   35 echo "need to compile WRF twice..."
   36 ./compile em_real &> log.em_real2
   37 cd ../

If the code is compiled, *real.exe*, *wrf.exe*, *ndown.exe*, and *tc.exe* can
be seen in the folder WRFV3911_ao/main. Here, *3911* means WRF3911, *ao* means
coupled ocean--atmosphere code. To test the installation of the coupled
WRF--ESMF code, a simple test case is available in the :ref:`WRF test, real
cases<test_wrf_real>`.

Other guidance to compile WRF
-----------------------------

There is another guidance to compile WRF available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php

