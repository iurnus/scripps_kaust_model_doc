.. _install_wrf:

###########
Install WRF
###########

How to get WRF code
===================

The WRF code is available at:

http://www2.mmm.ucar.edu/wrf/users/download/get_source.html

Register an account, then download the WRF-ARW code, WPS code, WRFDA/WRFPLUS code, and WRF-Chemistry
code. Currently we are using WRF 3.9.1.1.

Install WRF coupled with ESMF, using NetCDF4 and OpenMPI
========================================================

Open the tar file
-----------------

After downloading the WRF 3.9.1.1 (version 3.9 or 3.8 should also be OK), open
the file in *$HOME/scripps_kaust_model/*::

    tar -xf WRFV3.9.1.1.TAR.gz

Make a copy of the folder::

    cp -rf $HOME/scripps_kaust_model/WRFV3 $HOME/scripps_kaust_model/WRFV3911_AO


Configurations
--------------
Enter the WRF folder::

    cd $HOME/scripps_kaust_model/WRFV3911_AO

Run the configuration file::

   ./configure

You will see 75 options::

Please select from among the following Linux x86_64 options::

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
  13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

We enter '54' to activate PGI (pgf90/pgcc) + dmpar (distributed memory)::

  Enter selection [1-75] : 54

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

We can see a configuration file in the WRF folder::

  -rw-rw-r-- 1 ruisun ruisun 20823 2019-08-21 15:45 configure.wrf

Next, we need to edit this configuration file.
  
At the beginning of *configure.wrf* (line 16 in my file), add the ESMF_DIR.
Note that $HOME must be the real path of the home directory::

  ESMF_DIR=$HOME/scripps_kaust_model/esmf/

Replace the ESMF switches in *configure.wrf* (from line 76 to 96 in my file)::

  #### ESMF switches                 ####
  #### These are set up by Config.pl ####
  # switch to use separately installed ESMF library for coupling:  1==true
  ESMF_COUPLING       = 2
  # select dependences on module_utility.o
  ESMF_MOD_DEPENDENCE = $(WRF_SRC_ROOT_DIR)/external/io_esmf/module_utility.o
  # select -I options for external/io_esmf vs. external/esmf_time_f90
  ESMF_IO_INC         = -I$(WRF_SRC_ROOT_DIR)/external/io_esmf
  # select -I options for separately installed ESMF library, if present
  ESMF_MOD_INC        = -I$(ESMF_DIR)/mod/modg/Linux.pgi.64.openmpi.default -I$(WRF_SRC_ROOT_DIR)/main $(ESMF_IO_INC)
  # select cpp token for external/io_esmf vs. external/esmf_time_f90
  ESMF_IO_DEFS        = -DESMFIO
  # select build target for external/io_esmf vs. external/esmf_time_f90
  ESMF_TARGET         = wrfio_esmf
  # add the lib of ESMF
  ESMFLIB         = $(ESMF_DIR)/lib/libg/Linux.pgi.64.openmpi.default
  include $(ESMFLIB)/esmf.mk

Add *-DESMFIO* in *ARCHFLAGS* (line 164 in my file). Put it below *-DNETCDF \/*  ::

  -DNETCDF \
  -DESMFIO \

Modify the ESMF flags, replace (from line 192 to line 195 in my file)::

  ESMF_LIB_FLAGS  =    $(ESMF_LDFLAG)
  ESMF_IO_LIB     =    $(ESMF_F90LINKPATHS) $(ESMF_F90ESMFLINKLIBS) -L$(WRF_SRC_ROOT_DIR)/external/io_esmf -lwrfio_esmf
  ESMF_IO_LIB_EXT =    $(ESMF_IO_LIB)

Add ESMF in *INCLUDE_MODULES* (from line 196 in my file)::

  -I$(ESMF_DIR)/mod/modg/Linux.pgi.64.openmpi.default \

Add ESMF_DIR in *LIB_EXTERNAL* (from line 221 in my file)::
  LIB_EXTERNAL = -L$(ESMF_DIR)/lib/libg/Linux.pgi.64.openmpi.default -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/usr/local/netcdf/432_pgi133//lib -lnetcdff -lnetcdf

Install WRF
-----------

A few other files need to be copied before installing WRF (current folder $HOME/scripps_kaust_model/WRFV3911_AO/)::

   WRF_OPTION_DIR=../installOption_WRF/wrfAO3911Implementations_ring/
   ln -sf ${WRF_OPTION_DIR}/Makefile.main main/Makefile
   ln -sf ${WRF_OPTION_DIR}/wrf_test_ESMF.F main/
   ln -sf ${WRF_OPTION_DIR}/Makefile.wrf Makefile
   ln -sf ${WRF_OPTION_DIR}/makefile.io_netcdf external/io_netcdf/makefile
   ln -sf ${WRF_OPTION_DIR}/makefile.io_esmf external/io_esmf/makefile
   ln -sf ${WRF_OPTION_DIR}/module_domain.F frame/
   ln -sf ${WRF_OPTION_DIR}/ext_esmf_write_field.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/ext_esmf_read_field.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/ext_esmf_open_for_read.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/ext_esmf_open_for_write.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/module_esmf_extensions.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/module_diag_rasm.F phys/
   ln -sf ${WRF_OPTION_DIR}/module_diag_cl.F phys/
   ln -sf ${WRF_OPTION_DIR}/module_diagnostics_driver.F phys/
   ln -sf ${WRF_OPTION_DIR}/io_esmf.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR}/Registry.EM Registry/Registry.EM
   ln -sf ${WRF_OPTION_DIR}/Registry.EM_COMMON_direct Registry/Registry.EM_COMMON

Need to check the makefile for io_esmf (external/io_esmf/makefile).
   
Start of the file::

   ESMF_DIR=~/scripps_kaust_model/esmf/
   ESMF_INCLUDE  = ${ESMF_DIR}/mod/modg/Linux.pgi.64.openmpi.default/
   ESMF_LIBRARY  = ${ESMF_DIR}/lib/libg/Linux.pgi.64.openmpi.default/
   MAIN_DIR=~/scripps_kaust_model/
   CURRENT_DIR=~/scripps_kaust_model/WRFV3911_AO/external/io_esmf/
   NETCDF_INCLUDE=/usr/local/netcdf/432_pgi133/include/

Line 35 needs *NETCDF_INCLUDE* sometimes::

   $(FC) -I$(ESMF_INCLUDE) -I$(MAIN_DIR) -I$(CURRENT_DIR) -I$(NETCDF_INCLUDE) -c -g -I../ioapi_share $*.f

Also need to check the makefile for io_netcdf (external/io_netcdf). Line
7,8,46,48 need to be updated with the real path of netcdf::

  LIBS    = $(LIB_LOCAL) -L$(NETCDFPATH)/lib -L/usr/local/netcdf/432_pgi133/lib -lnetcdf
  LIBFFS  = $(LIB_LOCAL) -L$(NETCDFPATH)/lib -L/usr/local/netcdf/432_pgi133/lib -lnetcdf

  $(CPP1) -I/usr/local/netcdf/432_pgi133/include -I$(NETCDFPATH)/include -I../ioapi_share diffwrf.F90 | sed '/integer *, *external.*iargc/d' > diffwrf.f ;\
  $(CPP1) -I/usr/local/netcdf/432_pgi133/include -I$(NETCDFPATH)/include -I../ioapi_share diffwrf.F90 > diffwrf.f ; \


Now we can start compiling WRF by using (current folder *$HOME/scripps_kaust_model/WRFV3911_AO/*)::

  ./compile em_real &> log.em_real &

Need to compile two times. The first compile will not be successful because *io_esmf* is not successfully compiled.

After WRF is successfully compiled, you will see a few \*.exe files::

  $ ls -l main/*.exe
  -rwxrwxr-x 1 ruisun ruisun 70086798 2019-08-01 05:00 main/ndown.exe
  -rwxrwxr-x 1 ruisun ruisun 62036118 2019-08-01 05:00 main/real.exe
  -rwxrwxr-x 1 ruisun ruisun 61985460 2019-08-01 05:00 main/tc.exe
  -rwxrwxr-x 1 ruisun ruisun 68344825 2019-08-01 05:00 main/wrf.exe

Other guidance to compile WRF
-----------------------------

There is another guidance to compile WRF available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php

