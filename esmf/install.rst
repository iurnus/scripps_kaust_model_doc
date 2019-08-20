############
Install ESMF
############

Download ESMF
=============

ESMF is available at:

    https://www.earthsystemcog.org/projects/esmf/download/

The earlier versions of ESMF can be found at:

    http://www.earthsystemmodeling.org/download/data/releases.shtml

Install ESMF Using PGI compiler
===============================

First, download ESMF version 7.0.0 from the ESMF website. 

Clone the git repository of the coupled model::

  git clone https://github.com/iurnus/scripps_kaust_model.git

Check the folder of the coupled model::

  cd scripps_kaust_model
  ls

The following folders and files will be shown::

  -rwxrwxr-x  1 ruisun ruisun  132 2019-07-31 07:19 Allclean.sh
  -rwxrwxr-x  1 ruisun ruisun 4323 2019-08-01 02:35 Allmake.ring.sh
  -rwxrwxr-x  1 ruisun ruisun 4106 2019-07-31 07:25 Allmake.shaheen.sh
  drwxrwxr-x 13 ruisun ruisun 4096 2019-08-02 01:36 coupler
  drwxrwxr-x  4 ruisun ruisun   59 2019-07-31 07:19 esmf_test_application
  drwxrwxr-x  5 ruisun ruisun 4096 2019-07-31 15:25 installOption_OTH
  drwxrwxr-x 11 ruisun ruisun 4096 2019-08-01 03:25 installOption_WRF
  drwxrwxr-x  2 ruisun ruisun   52 2019-07-31 07:19 license_statements
  -rw-rw-r--  1 ruisun ruisun 2430 2019-07-31 07:19 README.md

Load the modules
================

Load the following modules::

  # load pgi compiler, openmpi, netcdf
  module load pgi13/compiler-13.3
  module load pgi13/openmpi-2.0.2
  module load pgi13/netcdf-4.3.2

  # change the path of pgi compiler, openmpi, netcdf
  export NETCDF_ROOT="/usr/local/netcdf/432_pgi133/"
  export MPI_INC_DIR="/usr/local/mpi/pgi13/openmpi-2.0.2/include/"
  export LD_LIBRARY_PATH=/usr/local/pgi/133/linux86-64/13.3/libso/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=/usr/local/mpi/pgi13/openmpi-2.0.2/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=/usr/local/netcdf/432_pgi133/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export NETCDF=/usr/local/netcdf/432_pgi133/

  # change the path of ESMF
  export LD_LIBRARY_PATH=/home/ruisun/scripps_kaust_model/esmf/lib/libg/Linux.pgi.64.openmpi.default/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

Note that:
  (1) the modules have different names for different machines, please load the modules on your machine
  (2) the paths are also different for different machines
  (3) the path of ESMF should be the path of *libesmf.a* and *esmf.mk* after ESMF is successfully installed

Configure ESMF
==============

I am using the following ESMF configurations::

  # compile options
  export ESMF_DIR=$(pwd)/
  export ESMFMKFILE=${ESMF_DIR}lib/libg/Linux.pgi.64.openmpi.default/esmf.mk
  echo "ESMF DIR is: $ESMF_DIR"
  echo "ESMFMKFILE is: $ESMFMKFILE"
  export ESMF_OS=Linux
  export ESMF_COMPILER=pgi
  export ESMF_COMM=openmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=split
  export ESMF_BOPT=g
  export ESMF_SITE=default
  export ESMF_ABI=64
  export ESMF_NETCDF_INCLUDE="/usr/local/netcdf/432_pgi133/include/"
  export ESMF_NETCDF_LIBPATH="/usr/local/netcdf/432_pgi133/lib/"
  export ESMF_NETCDF_LIBPATH_PREFIX="-Wl,-rpath,/usr/local/netcdf/432_pgi133/lib/"

  # test options
  export ESMF_TESTMPMD=OFF
  export ESMF_TESTHARNESS_ARRAY=RUN_ESMF_TestHarnessArray_default
  export ESMF_TESTHARNESS_FIELD=RUN_ESMF_TestHarnessField_default
  export ESMF_TESTWITHTHREADS=OFF
  export ESMF_TESTEXHAUSTIVE=ON

Note that:
  (1) *ESMF_NETCDF_INCLUDE*, *ESMF_NETCDF_LIBPATH*, *ESMF_NETCDF_LIBPATH_PREFIX* should be modified according to the NETCDF setups. 
  (2) *ESMF_COMPILER=pgi* means I am using PGI compiler. When using other compilers, one needs to change this option.
  (3) The explaination of other configurations is documented in ESMF tutorials.

Compile ESMF
============
Check the information of necessary configurations::

    gmake info

Compile the code::

    gmake
 
If it is the first time ESMF is installed, make sure to test ESMF using::

    gmake all_tests

If ESMF7.0.0 is successfully built, the unit tests will pass.

The perfect build summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_7_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/700/700_PC-Xeon-Cluster_Discover_PGI.html
