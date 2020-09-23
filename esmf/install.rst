######################
Install ESMF (general)
######################

Download ESMF
=============

ESMF 8.0.0 is available at:

    https://sourceforge.net/p/esmf/esmf/ci/ESMF_8_0_0/tree/

The earlier releases of ESMF can be found at:

    http://www.earthsystemmodeling.org/download/data/releases.shtml

Install ESMF Using PGI compiler
===============================

Install step 2: Download ESMF 8.0.0 (current working directory: $HOME/scripps_kaust_model-1.1)::

  git clone https://git.code.sf.net/p/esmf/esmf esmf
  cd esmf
  git checkout ESMF_8_0_0

You can find a message::

  HEAD is now at f5d862d... Update the supported platform table for the 8.0.0 release.


Load the modules
================

Install step 3: Load PGI compiler, OpenMPI and NetCDF (current working directory:
$HOME/scripps_kaust_model-1.1/esmf)::

  # load pgi compiler, openmpi, netcdf
  module load pgi13/compiler-13.3
  module load pgi13/openmpi-2.0.2
  module load pgi13/netcdf-4.3.2

  # update the path of pgi compiler, openmpi, netcdf
  export NETCDF_DIR=/usr/local/netcdf/432_pgi133/
  export MPI_DIR=/usr/local/mpi/pgi13/openmpi-2.0.2/

  export SKRIPS_DIR=$HOME/scripps_kaust_model-1.1/
  export SKRIPS_NETCDF_INCLUDE=-I$NETCDF_DIR/include/
  export SKRIPS_NETCDF_LIB=-L$NETCDF_DIR/lib/
  export SKRIPS_MPI_DIR=$MPI_DIR

Usually, I put them in the ~/.bashrc file to reuse them. The variables *SKRIPS_DIR*,
*SKRIPS_NETCDF_INCLUDE*, *SKRIPS_NETCDF_LIB*, *SKRIPS_MPI_DIR* will be used to build the coupled
model.

Note:
  1. The modules (pgi compiler, openmpi, netcdf) have different names for different machines, please
     load the modules on your machine.

  2. The paths are also different for different machines.

  3. In the last line, the path of ESMF should be the path of *libesmf.a* and *esmf.mk* after ESMF
     is successfully installed.

Configure ESMF
==============

I am using a few specific configurations to compile ESMF. 

Install step 4: Set ESMF configurations (current working directory: 
$HOME/scripps_kaust_model-1.1/esmf)::

  # ESMF compile options
  export ESMF_DIR=$SKRIPS_DIR/esmf/
  export ESMF_LIB=$ESMF_DIR/lib/libg/Linux.intel.64.openmpi.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk
  export ESMF_OS=Linux
  export ESMF_COMPILER=pgi
  export ESMF_COMM=openmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=split
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_YAMLCPP=OFF

  export ESMF_NETCDF_INCLUDE=$NETCDF_DIR/include/
  export ESMF_NETCDF_LIBPATH=$NETCDF_DIR/lib/
  export ESMF_NETCDF_LIBPATH_PREFIX="-Wl,-rpath,$NETCDF_DIR/lib/"

  # add the path of libraries...
  export LD_LIBRARY_PATH=$NETCDF_DIR/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$MPI_DIR/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$ESMF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

Again, I put them in the ~/.bashrc file.

Note:
  1. Sometimes NETCDF is compiled separately or PNETCDF is used. If so, *ESMF_NETCDF_INCLUDE*,
  *ESMF_NETCDF_LIBPATH*, *ESMF_NETCDF_LIBPATH_PREFIX* should be modified according to NETCDF
  setups. 

  2. *ESMF_COMPILER=pgi* means I am using PGI compiler. When using other compilers, one needs to
  change this option.

  3. The explaination of other configurations is documented in ESMF tutorials.

  4. If build_rules.mk file is not modified, you may still compile ESMF successfully. However, you
  will fail when compiling the coupled code.

Compile ESMF
============

Install step 5: Compile ESMF (current working directory: $HOME/scripps_kaust_model-1.1/esmf)::

    # Check the information of necessary configurations
    gmake info &> log.info

    # Compile the code
    gmake &> log.gmake

    # If it is the first time ESMF is installed, make sure to test ESMF using::
    gmake all_tests &> log.all_tests

If ESMF8.0.0 is successfully built, all the unit tests should pass. We can also compile the coupled
code when a few unit tests failed. On ESMF official website, some unit tests could also fail.
Currently we don't know which specific tests must pass for the coupled code.

The perfect build summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_8_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/800/800_Discover_pgi-17.7.0.html
