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


Modify the bashrc file
======================

At this step, we need to load the modules and set the system variables to install SKRIPS. 
**Open the ~/.bashrc file**::

  vim ~/.bashrc

Install step 3.1: In the ~/.bashrc file, add the following lines to load PGI compiler, 
OpenMPI and NetCDF (current working directory: $HOME/scripps_kaust_model-1.1/esmf)::

  # load pgi compiler, openmpi, netcdf
  module load pgi13/compiler-13.3
  module load pgi13/openmpi-2.0.2
  module load pgi13/netcdf-4.3.2

  # update the path of pgi compiler, openmpi, netcdf
  export SKRIPS_DIR=$HOME/scripps_kaust_model-1.1/
  export NETCDF_DIR=/usr/local/netcdf/432_pgi133/
  export NETCDF_INC=/usr/local/netcdf/432_pgi133/include/
  export NETCDF_LIB=/usr/local/netcdf/432_pgi133/lib/
  export MPI_DIR=/usr/local/mpi/pgi13/openmpi-2.0.2/
  export MPI_LIB=/usr/local/mpi/pgi13/openmpi-2.0.2/lib/
  export ESMF_DIR=$SKRIPS_DIR/esmf/
  export WRF_DIR=$SKRIPS_DIR/WRF412_AO/
  export MITGCM_DIR=$SKRIPS_DIR/MITgcm_c67m/

Note:
  1. Please make sure all these paths are correct. We need to find the SKRIPS model, 
     the netcdf header files and libraries, the MPI header files and libraries. They are all very important.

  2. The modules (MPI, NETCDF) have different names for different machines, please
     load the modules on your machine.

  3. The paths are also different for different machines.

Install step 3.2: In the ~/.bashrc file, set ESMF configurations (current working directory: 
$HOME/scripps_kaust_model-1.1/esmf)::

  # ESMF compile options
  export ESMF_OS=Linux
  export ESMF_COMPILER=pgi
  export ESMF_COMM=openmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=split
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_YAMLCPP=OFF

Notes::

  1. *ESMF_COMPILER=pgi* means I am using PGI compiler. 
  2. *ESMF_COMM=openmpi* means I am using OpenMPI. 
  3. The explaination of other configurations is documented in ESMF tutorials.

Install step 3.3: In the ~/.bashrc file, set the variables used in SKRIPS model and ESMF 
(current working directory: $HOME/scripps_kaust_model-1.1/esmf)::
  
  export SKRIPS_NETCDF_INCLUDE=-I$NETCDF_INC
  export SKRIPS_NETCDF_LIB=-L$NETCDF_LIB
  export SKRIPS_MPI_DIR=$MPI_DIR
  
  export ESMF_LIB=$ESMF_DIR/lib/lib$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk
  export ESMF_NETCDF_INCLUDE=$NETCDF_INC
  export ESMF_NETCDF_LIBPATH=$NETCDF_LIB
  export ESMF_NETCDF_LIBPATH_PREFIX="-Wl,-rpath,$NETCDF_LIB"
  
  export LD_LIBRARY_PATH=$NETCDF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$MPI_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$ESMF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}

Finally, **Close the ~/.bashrc file and activate the changes**::

  source ~/.bashrc

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

The complete summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_8_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/800/800_Discover_pgi-17.7.0.html
