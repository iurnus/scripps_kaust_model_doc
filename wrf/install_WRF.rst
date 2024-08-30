.. _install_wrf:

###########
Install WRF
###########

Install WRF on COMET
====================

We have prepared the installer for install WRF on kala/Shaheen/expanse.

Install step 4.1: Download WRF::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WRF/archive/v4.5.1.zip
  unzip v4.5.1.zip
  mv WRF-4.5.1 WRFV451_AO
  # save a copy
  cp -rf WRFV451_AO WRFV451_AO.org

Step 4.2: Run the installer::
  
  cd scripts/wrf/
  ## FOR SHAHEEN
  ## ./installWRF451_ao_shaheen.sh
  ## FOR EXPANSE
  ## ./installWRF451_ao_expanse.sh
  ## FOR KALA
  ./installWRF451_ao_kala.sh

In the installer, we have updated the WRF configurations files and generated
the scripts. However, we did not provide this for other computers. The user
must update the WRF configurations related to ESMF setups following the
instruction below.  

Install WRF on other machines
=============================

Download and configure WRF
--------------------------

Install step 4.1: Download WRF::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WRF/archive/v4.5.1.zip
  unzip v4.5.1.zip
  mv WRF-4.5.1 WRFV451_AO
  # save a copy
  cp -rf WRFV451_AO WRFV451_AO.org

Install step 4.2: Set the WRF configurations::
  
  cd WRFV413_AO
  ./configure

There are 79 WRF configurations.

Please select from among the following Linux x86_64 options::

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
  13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

I use option '34' to activate Intel compiler on COMET (use 15 for expanse; use 50 for shaheen)::

  Enter selection [1-75] : 34

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

We can see a configuration file in the WRF folder::

  -rw-r--r--  1 rus043 mazloff-lab 21185 Aug 30 13:05 configure.wrf

Install step 4.3: Edit the WRF configurations for the coupled model:

At the beginning of *configure.wrf* (line 17 in my file), add the *ESMF_DIR*,
which is the path of the ESMF directory::

  ESMF_DIR=$SKRIPS_DIR/esmf/

Replace the ESMF switches in *configure.wrf* (from line 76 to 96 in my file).
Check ESMF path *ESMF_MOD* and *ESMF_LIB* defined in *bashrc\_skrips* (in Step
1)::

  #### ESMF switches                 ####
  #### These are set up by Config.pl ####
  # switch to use separately installed ESMF library for coupling:  1==true
  ESMF_COUPLING       = 2
  # select dependences on module_utility.o
  ESMF_MOD_DEPENDENCE = $(WRF_SRC_ROOT_DIR)/external/io_esmf/module_utility.o
  # select -I options for external/io_esmf vs. external/esmf_time_f90
  ESMF_IO_INC         = -I$(WRF_SRC_ROOT_DIR)/external/io_esmf
  # select -I options for separately installed ESMF library, if present
  ESMF_MOD_INC        = -I$(ESMF_MOD) -I$(WRF_SRC_ROOT_DIR)/main -I$(WRF_SRC_ROOT_DIR)/frame $(ESMF_IO_INC)
  # select cpp token for external/io_esmf vs. external/esmf_time_f90
  ESMF_IO_DEFS        = -DESMFIO
  # select build target for external/io_esmf vs. external/esmf_time_f90
  ESMF_TARGET         = wrfio_esmf
  # add the lib of ESMF
  include $(ESMF_LIB)/esmf.mk

Add *-DESMFIO* in *ARCHFLAGS* (line 188 in my file). Put it below *-DNETCDF*::

  -DNETCDF \
  -DESMFIO \

Modify the ESMF flags, replace (from line 222 to line 223 in my file)::

  ESMF_IO_LIB     =    $(ESMF_F90LINKPATHS) $(ESMF_F90ESMFLINKLIBS) -L$(WRF_SRC_ROOT_DIR)/external/io_esmf -lwrfio_esmf
  ESMF_IO_LIB_EXT =    $(ESMF_IO_LIB)

Add *ESMF_LIB* in *LIB_EXTERNAL* (from line 124 in my file)::

  LIB_EXTERNAL = $(ESMF_LIB)/libesmf.a -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/home/rus043/libraries/netcdf-build-fortran/lib -lnetcdff

Add INCLUDE_MODULES when compiling io_esmf (line 372 in my file)::

  make FC="$(FC) $(PROMOTION) $(FCDEBUG) $(FCBASEOPTS) $(ESMF_MOD_INC) $(INCLUDE_MODULES)" \

Then, save *configure.wrf* file after the edit.

Finally, the coupler need these WRF variables. Now save the variables of the
configuration file in another file (current working directory:
$SKRIPS_DIR/WRFV413_AO)::

  linenumber=$(grep -n "bundled:" configure.wrf | cut -d : -f 1)
  head -n $((linenumber-1)) configure.wrf > configure.wrf_cpl

The generated *configure.wrf_cpl* file will be used to compile the coupled
model.

Compile WRF
-----------

Install step 4.4: Copy other files and install WRF (current working directory:
$SKRIPS_DIR/WRFV413_AO)::

   WRF_OPTION_DIR0=$SKRIPS_DIR/scripts/wrf/wrfAO451_shared/

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
  -rwxr-xr-x 1 rus043 mazloff-lab 47660776 Aug 30 14:47 main/ndown.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 47726416 Aug 30 14:47 main/real.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 46968840 Aug 30 14:47 main/tc.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 55731328 Aug 30 14:45 main/wrf.exe


Other guidance to compile WRF
=============================

There is another guidance to compile WRF available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php


Known issues
============

(1) By default WRF outputs the relative wind speed. To get the absolute wind
speed, one must add the relative wind to ocean current velocity in the coupled
simulations.

(2) In the coupled model, we used RRTMG because it can output the surface
radiative fluxes. Other options do not explicitly output the surface radiative
fluxes.

(3) The WRF output to ESMF uses auxhist5 (auxiliary history output 5) in WRF.
It is in conflict with the diagnostics of Regional Arctic System Model (RASM)
in WRF.

