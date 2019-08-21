.. _install_cpl:

####################
Install coupled code
####################

How to install
==============

After compiling all three model components, we can try compile the coupled code::

  cd $HOME/scripps_kaust_model/couplers
  ls -l

We can see::

  -rwxrwxr-x  1 ruisun ruisun  438 2019-08-01 08:02 allrun.ring.sh
  drwxrwxr-x  6 ruisun ruisun  112 2019-08-01 08:02 L1.C1.mitgcm_case_CA2009
  drwxrwxr-x  4 ruisun ruisun   81 2019-08-01 08:02 L1.C2.esmf_coupled_test
  drwxrwxr-x  6 ruisun ruisun  110 2019-08-01 08:02 L2.C1.mitgcm_case_CA2009_mpiESMF_1cpu
  drwxrwxr-x  4 ruisun ruisun   78 2019-08-01 08:02 L2.C2.mitgcm_case_CA2009_mpiESMF_2cpu
  drwxrwxr-x 13 ruisun ruisun 4096 2019-08-01 08:02 L3.C1.coupled_RS2012_ring
  drwxrwxr-x 12 ruisun ruisun 4096 2019-08-01 08:02 L3.C2.coupled_CA2018_ring
  drwxrwxr-x 12 ruisun ruisun 4096 2019-08-01 08:02 L3.C2.coupled_CA2018_shaheen
  -rw-rw-r--  1 ruisun ruisun 1369 2019-08-01 08:02 README
  drwxrwxr-x  2 ruisun ruisun 4096 2019-08-01 08:02 script-post

Enter folder *L3.C1.coupled_RS2012_ring*::

  cd L3.C1.coupled_RS2012_ring/
  ls -l

We can see (current folder *$HOME/scripps_kaust_model/couplers/L3.C1.coupled_RS2012_ring/*)::

  -rwxrwxr-x 1 ruisun ruisun  191 2019-08-01 08:02 allclean.sh
  -rwxrwxr-x 1 ruisun ruisun  241 2019-08-01 08:02 allrun.sh
  drwxrwxr-x 2 ruisun ruisun 4096 2019-08-01 08:02 caseInput
  -rwxrwxr-x 1 ruisun ruisun  159 2019-08-01 08:02 clean.sh
  drwxrwxr-x 2 ruisun ruisun 4096 2019-08-01 08:02 coupledCode
  -rwxrwxr-x 1 ruisun ruisun 1274 2019-08-01 08:02 install.sh
  drwxrwxr-x 4 ruisun ruisun   99 2019-08-01 08:02 latexSummary
  drwxrwxr-x 2 ruisun ruisun 4096 2019-08-01 08:02 mitCode
  drwxrwxr-x 2 ruisun ruisun  107 2019-08-01 08:02 mitSettingRS
  -rw-rw-r-- 1 ruisun ruisun 1480 2019-08-01 08:02 README.install
  drwxrwxr-x 2 ruisun ruisun  147 2019-08-01 08:02 runCase
  drwxrwxr-x 2 ruisun ruisun   68 2019-08-01 08:02 runCase.init
  drwxrwxr-x 3 ruisun ruisun   62 2019-08-01 08:02 runMITtest
  drwxrwxr-x 2 ruisun ruisun  111 2019-08-01 08:02 runWRFtest
  drwxrwxr-x 2 ruisun ruisun   86 2019-08-01 08:02 save_nc
  drwxrwxr-x 2 ruisun ruisun  151 2019-08-01 08:02 utils

Update utils/mitgcm_options (line 28 and 29):: 

  INCLUDES='-I/usr/local/netcdf/432_pgi133/include -I/usr/local/mpi/pgi13/openmpi-2.0.2/include/'
  LIBS='-L/usr/local/netcdf/432_pgi133/lib/ -L/usr/local/mpi/pgi13/openmpi-2.0.2/lib/'

Update utils/mkmod.sh (from line 217):: 

  set comp     = /usr/local/mpi/pgi13/openmpi-2.0.2/bin/mpif77
  set cccommand = /usr/local/mpi/pgi13/openmpi-2.0.2/bin/mpicc
  set compopts = (-byteswapio -r8 -Mnodclchk -Mextend -fast -fastsse)
  set compopts_num = ( $compopts )
  set complibs = (-L/usr/local/netcdf/432_pgi133/lib -lnetcdf)
  set compinc = (-I/usr/local/netcdf/432_pgi133/include)

Update utils/install.sh (line 2)::

  export MPI_HOME="/usr/local/mpi/pgi13/openmpi-2.0.2/include/"

Update coupledCode/wrflib.mk (replace line 24 to 28)::

  -L/usr/local/netcdf/432_pgi133/lib \
  -L/opt/pgi/linux86-64/17.5/libso \
  -lesmf  -lmpi -pgcpplibs -ldl -lnetcdff -lnetcdf \

Run install.sh ::

  ./install.sh

Enter the correct PATH::

  WRF3911 (with OA coupling) location? :/home/ruisun/scripps_kaust_model/coupler/L3.C1.coupled_RS2012_ring/../../WRFV3911_AO/
  ESMF location? :/home/ruisun/scripps_kaust_model/coupler/L3.C1.coupled_RS2012_ring/../../esmf/

The installer will generate *esmf_application* in folder coupledCode folder (current folder *$HOME/scripps_kaust_model/couplers/L3.C1.coupled_RS2012_ring/*)::

  [ruisun@acc00]~/scripps_kaust_model/coupler/L3.C1.coupled_RS2012_ring$ cd coupledCode/
  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/coupledCode$ ls
  Allmake.sh           mod_config.F90    mod_esmf_esm.F90  module_wrf_top.o
  esmf_application     mod_config.mod    mod_esmf_esm.mod  run_me_first.sh
  libmitgcm_org_ocn.a  mod_config.o      mod_esmf_esm.o    setrlstk.o
  libmitgcmrtl.a       mod_esmf_atm.F90  mod_esmf_ocn.F90  sigreg.o
  libwrflib.a          mod_esmf_atm.mod  mod_esmf_ocn.mod  wrf_ESMFMod.o
  Makefile             mod_esmf_atm.o    mod_esmf_ocn.o    wrflib.mk
  mitgcm_org_ocn.mod   mod_esmf_cpl.F90  mod_types.F90
  mitgcm_wrf.F90       mod_esmf_cpl.mod  mod_types.mod
  mitgcm_wrf.o         mod_esmf_cpl.o    mod_types.o


Known errors
============

1. pgfortran-Error-Unknown switch: -pgc++libs

This is because the name of the pgi libraries are different from ring@ucsd.edu, I will replace *-pgc++libs* using *-pgcpplibs* in coupledCode/wrflib.mk to avoild this error.


2. PGF90-W-0025-Illegal character (#) - ignored (mod_esmf_atm.F90: 564)

This is because the include function in mod_esmf_atm.F90 is illegal. I will replace *#include "med_find_esmf_coupling.inc"* using *include "med_find_esmf_coupling.inc"*.
