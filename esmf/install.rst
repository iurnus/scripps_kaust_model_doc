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

Install step 2: Download ESMF 8.0.0 (current folder: $HOME/scripps_kaust_model-1.1)::

  git clone https://git.code.sf.net/p/esmf/esmf esmf
  cd esmf
  git checkout ESMF_8_0_0

You can find a message ::

  HEAD is now at f5d862d... Update the supported platform table for the 8.0.0 release.


Load the modules
================

Install step 3: Load PGI compiler, OpenMPI and NetCDF (current folder:
$HOME/scripps_kaust_model-1.1/esmf)::

  # load pgi compiler, openmpi, netcdf
  module load pgi13/compiler-13.3
  module load pgi13/openmpi-2.0.2
  module load pgi13/netcdf-4.3.2

  # update the path of pgi compiler, openmpi, netcdf
  export NETCDF=/usr/local/netcdf/432_pgi133/
  export NETCDF_ROOT=/usr/local/netcdf/432_pgi133/
  export MPI_INC_DIR=/usr/local/mpi/pgi13/openmpi-2.0.2/include/
  export LD_LIBRARY_PATH=/usr/local/pgi/133/linux86-64/13.3/libso/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=/usr/local/mpi/pgi13/openmpi-2.0.2/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=/usr/local/netcdf/432_pgi133/lib/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

  # add the path of ESMF
  export LD_LIBRARY_PATH=$HOME/scripps_kaust_model-1.1/esmf/lib/libg/Linux.pgi.64.openmpi.default/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

Usually, I put them in the ~/.bashrc file to reuse them.

Note:
  1. The modules (pgi compiler, openmpi, netcdf) have different names for different machines, please
     load the modules on your machine.

  2. The paths are also different for different machines.

  3. In the last line, the path of ESMF should be the path of *libesmf.a* and *esmf.mk* after ESMF
     is successfully installed.

Configure ESMF
==============

I am using a few specific configurations to compile ESMF. 

Install step 4: Set ESMF configurations (current folder
$HOME/scripps_kaust_model-1.1/esmf)::

  # ESMF compile options
  export ESMF_DIR=$HOME/scripps_kaust_model-1.1/esmf/
  export ESMFMKFILE=${ESMF_DIR}lib/libg/Linux.pgi.64.openmpi.default/esmf.mk
  export ESMF_OS=Linux
  export ESMF_COMPILER=pgi
  export ESMF_COMM=openmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=split
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_YAMLCPP=OFF
  export ESMF_NETCDF_INCLUDE=/usr/local/netcdf/432_pgi133/include/
  export ESMF_NETCDF_LIBPATH=/usr/local/netcdf/432_pgi133/lib/
  export ESMF_NETCDF_LIBPATH_PREFIX="-Wl,-rpath,/usr/local/netcdf/432_pgi133/lib/"

Again, I put them in the ~/.bashrc file.

Note:
  1. *ESMF_NETCDF_INCLUDE*, *ESMF_NETCDF_LIBPATH*, *ESMF_NETCDF_LIBPATH_PREFIX* should be modified
  according to the NETCDF setups. 

  2. *ESMF_COMPILER=pgi* means I am using PGI compiler. When using other compilers, one needs to
  change this option.

  3. The explaination of other configurations is documented in ESMF tutorials.

  4. If build_rules.mk file is not modified, you may still compile ESMF successfully. However, you
  will fail when compiling the coupled code.

Compile ESMF
============

Install step 5: Compile ESMF (current folder: $HOME/scripps_kaust_model-1.1/esmf)::

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
