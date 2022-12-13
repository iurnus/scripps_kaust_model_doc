.. _install_wrf:

###########
Install WRF
###########

Install WRF on Shaheen or COMET
===============================

To install WRF on Shaheen or COMET, it is much easier. First download WRF::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WRF/archive/v4.1.3.zip
  unzip v4.1.3.zip
  mv WRF-4.1.3 WRFV413_AO
  # save a copy
  cp -rf WRFV413_AO WRFV413_AO.org

Then run the installer::
  
  cd installOption_WRF
  ## FOR SHAHEEN
  ./installWRF413_ao_shaheen.sh
  ## FOR COMET
  ## ./installWRF413_ao_comet.sh
  ## FOR RING
  ## ./installWRF413_ao_ring.sh


Install WRF on other machines
=============================

Download and configure WRF
--------------------------

Install step 4.1: Download WRF::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WRF/archive/v4.1.3.zip
  unzip v4.1.3.zip
  mv WRF-4.1.3 WRFV413_AO
  # save a copy
  cp -rf WRFV413_AO WRFV413_AO.org

Install step 4.2: Set the WRF configurations::
  
  cd WRFV413_AO
  ./configure

There are 75 WRF configurations.

Please select from among the following Linux x86_64 options::

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
  13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

I use option '15' to activate Intel compiler on COMET (use 54 for ring; use 50 for shaheen)::

  Enter selection [1-75] : 15

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

We can see a configuration file in the WRF folder::

  -rw-rw-r-- 1 ruisun ruisun 20823 2019-08-21 15:45 configure.wrf

Install step 4.3: Edit the WRF configurations:

At the beginning of *configure.wrf* (line 17 in my file), add the ESMF_DIR.
Note *ESMF_DIR* must be the real path of the home directory::

  ESMF_DIR=$SKRIPS_DIR/esmf/

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
  ESMF_MOD_INC        = -I$(ESMF_MOD) -I$(WRF_SRC_ROOT_DIR)/main $(ESMF_IO_INC)
  # select cpp token for external/io_esmf vs. external/esmf_time_f90
  ESMF_IO_DEFS        = -DESMFIO
  # select build target for external/io_esmf vs. external/esmf_time_f90
  ESMF_TARGET         = wrfio_esmf
  # add the lib of ESMF
  include $(ESMF_LIB)/esmf.mk

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
directory: $SKRIPS_DIR/WRFV413_AO)::

  linenumber=$(grep -n "bundled:" configure.wrf | cut -d : -f 1)
  head -n $((linenumber-1)) configure.wrf > configure.wrf_cpl

The generated *configure.wrf_cpl* file will be used to compile the coupled model.

Compile WRF
-----------

Install step 4.4: Copy other files and install WRF (current working directory:
$SKRIPS_DIR/WRFV413_AO)::

   WRF_OPTION_DIR0=$SKRIPS_DIR/wrfAO413_shared/

   ln -sf ${WRF_UPDATE_DIR0}/Makefile.wrf Makefile
   ln -sf ${WRF_UPDATE_DIR0}/Registry.EM Registry/
   
   ln -sf ${WRF_UPDATE_DIR0}/ext_esmf_write_field.F90 external/io_esmf/
   ln -sf ${WRF_UPDATE_DIR0}/ext_esmf_read_field.F90 external/io_esmf/
   ln -sf ${WRF_UPDATE_DIR0}/ext_esmf_open_for_read.F90 external/io_esmf/
   ln -sf ${WRF_UPDATE_DIR0}/ext_esmf_open_for_write.F90 external/io_esmf/
   ln -sf ${WRF_UPDATE_DIR0}/module_esmf_extensions.F90 external/io_esmf/
   ln -sf ${WRF_UPDATE_DIR0}/io_esmf.F90 external/io_esmf/
   
   ln -sf ${WRF_UPDATE_DIR0}/module_diag_rasm.F phys/
   ln -sf ${WRF_UPDATE_DIR0}/module_ltng_iccg.F phys/
   ln -sf ${WRF_UPDATE_DIR0}/module_sf_ruclsm.F phys/
   ln -sf ${WRF_UPDATE_DIR0}/module_sf_sfclayrev.F phys/
   ln -sf ${WRF_UPDATE_DIR0}/module_surface_driver.F phys/
   ln -sf ${WRF_UPDATE_DIR0}/module_sf_mynn.F phys/
   
   ln -sf ${WRF_UPDATE_DIR0}/input_wrf.F share/
   ln -sf ${WRF_UPDATE_DIR0}/module_domain.F frame/
   ln -sf ${WRF_UPDATE_DIR0}/module_first_rk_step_part1.F dyn_em/
   ln -sf ${WRF_UPDATE_DIR0}/wrf_ESMFMod.F main/
 
Now we can start compiling WRF by using::

  ./compile em_real &> log.em_real &

After WRF is successfully compiled, you will see a few \*.exe files::

  $ ls -l main/*.exe
  -rwxrwxr-x 1 ruisun ruisun 70086798 2019-08-01 05:00 main/ndown.exe
  -rwxrwxr-x 1 ruisun ruisun 62036118 2019-08-01 05:00 main/real.exe
  -rwxrwxr-x 1 ruisun ruisun 61985460 2019-08-01 05:00 main/tc.exe
  -rwxrwxr-x 1 ruisun ruisun 68344825 2019-08-01 05:00 main/wrf.exe



Other guidance to compile WRF
=============================

There is another guidance to compile WRF available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php


