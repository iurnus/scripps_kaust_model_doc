.. _install_wrf:

###########
Install WRF
###########

Update WRF configurations
=========================

Install step 7: Download WRF v4.1.2 (current working directory:
$HOME/scripps_kaust_model-1.1/MITgcm_c67m)::

  cd ..
  wget https://github.com/wrf-model/WRF/archive/v4.1.2.zip
  unzip v4.1.2.zip
  mv WRF-4.1.2 WRFV412_AO
  # save a copy
  cp -rf WRFV412_AO WRFV412_AO.org

Install step 8: Set the WRF configurations::
  
  cd WRFV412_AO
  ./configure

There are 75 WRF configurations.

Please select from among the following Linux x86_64 options::

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
  13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

I use option '54' to activate PGI (pgf90/pgcc) + dmpar (distributed memory)::

  Enter selection [1-75] : 54

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

We can see a configuration file in the WRF folder::

  -rw-rw-r-- 1 ruisun ruisun 20823 2019-08-21 15:45 configure.wrf

Install step 9: Edit the WRF configurations.
(current working directory: $HOME/scripps_kaust_model-1.1/WRFV412_AO)

At the beginning of *configure.wrf* (line 17 in my file), add the ESMF_DIR.
Note *ESMF_DIR* must be the real path of the home directory::

  ESMF_DIR=$HOME/scripps_kaust_model-v1.1/esmf/

Replace the ESMF switches in *configure.wrf* (from line 76 to 96 in my file). Note that the ESMF
path in *ESMF_IO_INC* and *ESMF_LIB* should be updated accordingly::

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

Add *-DESMFIO* in *ARCHFLAGS* (line 169 in my file). Put it below *-DNETCDF*::

  -DNETCDF \
  -DESMFIO \

Modify the ESMF flags, replace (from line 198 to line 199 in my file)::

  ESMF_IO_LIB     =    $(ESMF_F90LINKPATHS) $(ESMF_F90ESMFLINKLIBS) -L$(WRF_SRC_ROOT_DIR)/external/io_esmf -lwrfio_esmf
  ESMF_IO_LIB_EXT =    $(ESMF_IO_LIB)

Add ESMF_DIR in *LIB_EXTERNAL* (from line 227 in my file)::

  LIB_EXTERNAL = $(ESMF_LIB)/libesmf.a -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/usr/local/netcdf/432_pgi133//lib -lnetcdff -lnetcdf

Add INCLUDE_MODULES when compiling io_esmf (line 372 in my file)::

  make FC="$(FC) $(PROMOTION) $(FCDEBUG) $(FCBASEOPTS) $(ESMF_MOD_INC) $(INCLUDE_MODULES)" \

Then, save *configure.wrf* file after the edit.

Finally, save part of the configuration file in another file (current working
directory: $HOME/scripps_kaust_model-1.1/WRFV412_AO)::

  linenumber=$(grep -n "bundled:" configure.wrf | cut -d : -f 1)
  head -n $((linenumber-1)) configure.wrf > configure.wrf_cpl

The generated *configure.wrf_cpl* file will be used to compile the coupled model.

Compile WRF
-----------

Install step 11: Copy other files (current working directory:
$HOME/scripps_kaust_model-1.1/WRFV412_AO)::

   WRF_OPTION_DIR0=$HOME/scripps_kaust_model-1.1/installOption_WRF/wrfAO412_shared/

   ln -sf ${WRF_OPTION_DIR0}/Makefile.wrf Makefile
   ln -sf ${WRF_OPTION_DIR0}/module_domain.F frame/
   ln -sf ${WRF_OPTION_DIR0}/module_diag_rasm.F phys/
   ln -sf ${WRF_OPTION_DIR0}/module_ltng_iccg.F phys/
   ln -sf ${WRF_OPTION_DIR0}/input_wrf.F share/

   ln -sf ${WRF_OPTION_DIR0}/ext_esmf_write_field.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/ext_esmf_read_field.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/ext_esmf_open_for_read.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/ext_esmf_open_for_write.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/module_esmf_extensions.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/io_esmf.F90 external/io_esmf/
   ln -sf ${WRF_OPTION_DIR0}/wrf_ESMFMod.F main/
   
Now we can start compiling WRF by using::

  ./compile em_real &> log.em_real &

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

