.. _install_wrf:

###########
Install WRF
###########

Install WRF on kala
===================

We have prepared the installer for install WRF on kala or SHAHEEN III or SDSC Expanse.

Install step 4.1(a): Download WRF::

  cd $SKRIPS_DIR

  wget https://github.com/wrf-model/WRF/releases/download/v4.7.1/v4.7.1.zip
  unzip v4.7.1.zip
  tar -xvf v4.7.1.tar
  mv WRFV4.7.1 WRFV471_AO
  # save a copy
  cp -rf WRFV471_AO WRFV471_AO.org

Step 4.2(a): Run the installer::
  
  cd scripts/wrf/
  ## FOR SHAHEEN
  ## ./installWRF452_ao_shaheen.sh
  ## FOR EXPANSE
  ## ./installWRF452_ao_expanse.sh
  ## FOR KALA
  ## ./installWRF452_ao_kala.sh
  ## FOR GENERAL MACHINE
  ./installWRF452_ao_general.sh

The installer will check the location of WRF::

  WRF DIR is: /home/rus043/scripps_kaust_model//WRFV471_AO/
  Continue with this WRF DIR? (Y/N): Y

Then it will ask you to select the WRF configurations. It is same as running
``./configure`` in WRF directory::

    Please select from among the following Linux x86_64 options:
    1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
    5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
    9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
    13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

For my Linux machine kala, I have NetCDF and other dependences installed using
GNU compiler. Then I use option ``34`` to activate GNU compiler (use 15 for
expanse; use 50 for shaheen)::

  Enter selection [1-75] : 34

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

After entering all these information for WRF. The installer will try installing
WRF on the machine with ESMF. After WRF is successfully compiled, you will see
a few executables::

  $ ls -l main/*.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 47660776 Aug 30 14:47 main/ndown.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 47726416 Aug 30 14:47 main/real.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 46968840 Aug 30 14:47 main/tc.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 55731328 Aug 30 14:45 main/wrf.exe

.. note::

  1. We can see a ``configure.wrf`` file in the WRF folder about the configurations.
  2. The log file saved as ``log.em_real`` in the WRF folder.


About the installer
===================

Because the coupled model uses ESMF libraries. We must compile ESMF altogether
with WRF. Although we don't want the user to make any changes, it could be
possible that the user need to edit the WRF configuration file and compile WRF
step by step. Here is the detailed instruction. 


Download and configure WRF
--------------------------

Install step 4.1(b): Download WRF::

  cd $SKRIPS_DIR
  wget https://github.com/wrf-model/WRF/releases/download/v4.5.2/v4.5.2.zip
  unzip v4.5.2.zip
  tar -xvf v4.5.2.tar
  mv WRF-4.5.2 WRFV452_AO
  # save a copy
  cp -rf WRFV452_AO WRFV452_AO.org

Install step 4.2.1(b): Set the WRF configurations::
  
  cd WRFV452_AO
  ./configure

After running ``./configure``, there are several WRF configurations for us to select
from::

    Please select from among the following Linux x86_64 options:
    1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
    5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
    9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
    13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)

For my Linux machine kala, I have NetCDF and other dependences installed using
GNU compiler. Then I use option ``34`` to activate GNU compiler (use 15 for
expanse; use 50 for shaheen)::

  Enter selection [1-75] : 34

For the nesting option, we use 1 (basic) in our test::

  Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 1

We can see a configuration file in the WRF folder::

  -rw-r--r--  1 rus043 mazloff-lab 21185 Aug 30 13:05 configure.wrf

Install step 4.2.2(b): Edit the WRF configurations for the coupled model (I am
using ``vim`` editor)::

  vim configure.wrf

At the beginning of ``configure.wrf`` (line 17 in my file), add the *ESMF_DIR*,
which is the path of the ESMF directory::

  ESMF_DIR=$SKRIPS_DIR/esmf/

Replace the ESMF switches in ``configure.wrf`` (from line 76 to 96 in my file).
Check ESMF path ``ESMF_MOD`` and ``ESMF_LIB`` defined in ``bashrc_skrips`` (in Step
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

Add ``-DESMFIO`` in ``ARCHFLAGS`` (line 188 in my file). I put it below ``-DNETCDF``::

  -DNETCDF \
  -DESMFIO \

Modify the ESMF flags ``ESMF_IO_LIB`` and ``ESMF_IO_LIB_EXT`` (from line 222 to line 223 in my file)::

  ESMF_IO_LIB     =    $(ESMF_F90LINKPATHS) $(ESMF_F90ESMFLINKLIBS) -L$(WRF_SRC_ROOT_DIR)/external/io_esmf -lwrfio_esmf
  ESMF_IO_LIB_EXT =    $(ESMF_IO_LIB)

Add ``$ESMF_LIB/libesmf.a`` in ``LIB_EXTERNAL`` (from line 124 in my file)::

  LIB_EXTERNAL = $(ESMF_LIB)/libesmf.a -L$(WRF_SRC_ROOT_DIR)/external/io_netcdf -lwrfio_nf -L/home/rus043/libraries/netcdf-build-fortran/lib -lnetcdff

.. note::
    At the end of ``LIB_EXTERNAL`` is the location of the NetCDF library. I am
    using ``-L/home/rus043/libraries/netcdf-build-fortran/lib`` because I installed
    NetCDF there. Please make sure the location is correct.

Add ``INCLUDE_MODULES`` when compiling ``io_esmf`` (line 372 in my file)::

  make FC="$(FC) $(PROMOTION) $(FCDEBUG) $(FCBASEOPTS) $(ESMF_MOD_INC) $(INCLUDE_MODULES)" \

Then, save ``configure.wrf`` file after the edit.

Finally, the coupler need these WRF variables. Now save the variables of the
configuration file in another file::

  cd $SKRIPS_DIR/WRFV452_AO

  linenumber=$(grep -n "bundled:" configure.wrf | cut -d : -f 1)
  head -n $((linenumber-1)) configure.wrf > configure.wrf_cpl

The generated ``configure.wrf_cpl`` file will be used to compile the coupled
model.

Install step 4.2.3(b): Copy other files and install WRF (current working directory:
$SKRIPS_DIR/WRFV452_AO)::
   
   # These lines are used to generate configure.wrf. 
   # They are commented out in the step-by-step instruction.
   # ln -sf ${WRF_UPDATE_DIR0}/Config.pl arch/
   # ln -sf ${WRF_UPDATE_DIR0}/preample arch/
   # ln -sf ${WRF_UPDATE_DIR0}/postample arch/
   # # WRF configure=34, then nesting=1
   # echo "choosing 34th option to compile WRF"
   # echo "nesting option is 1 (normal)"
   # printf '34\n1\n' | ./configure &> log.configure

   echo "copying other files to compile ESMF--WRF"
   # ln -sf ${WRF_UPDATE_DIR1}/configure.wrf configure.wrf
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

After WRF is successfully compiled, you will see the same executables::

  $ ls -l main/*.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 47660776 Aug 30 14:47 main/ndown.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 47726416 Aug 30 14:47 main/real.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 46968840 Aug 30 14:47 main/tc.exe
  -rwxr-xr-x 1 rus043 mazloff-lab 55731328 Aug 30 14:45 main/wrf.exe


Other guidance to compile WRF
=============================

The guidance to compile WRF is available at:
http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compilation_tutorial.php

Known issues
============

(1) By default WRF outputs the relative wind speed. To get the absolute wind
speed, one must add the relative wind to ocean current velocity in the coupled
simulations.

(2) In the coupled model, we only support RRTMG radiation scheme because it can
output the surface radiative fluxes. Other options do not explicitly output the
surface radiative fluxes.

(3) The WRF output to ESMF uses auxhist5 (auxiliary history output 5) in WRF.
It is in conflict with the diagnostics of Regional Arctic System Model (RASM)
in WRF.

