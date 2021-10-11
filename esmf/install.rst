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

Install step 3.1: Download ESMF 8.0.0::

  cd $SKRIPS_DIR
  git clone https://git.code.sf.net/p/esmf/esmf esmf
  cd esmf
  git checkout ESMF_8_0_0

You can find a message::

  HEAD is now at f5d862d... Update the supported platform table for the 8.0.0 release.


Examine the bashrc file
=======================

Install step 3.2: In the ~/.bashrc_skrips file, the ESMF configurations (current working directory: 
$HOME/scripps_kaust_model/esmf)::

  # ESMF compile options
  export ESMF_OS=Linux
  export ESMF_COMPILER=intel
  export ESMF_COMM=intelmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=nc-config
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_YAMLCPP=OFF
  export ESMF_LIB=$ESMF_DIR/lib/lib$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk

Notes::

  1. *ESMF_COMPILER=intel* means I am using Intel compiler. 
  2. *ESMF_COMM=intelmpi* means I am using Intel MPI. 
  3. The explaination of other configurations is documented in ESMF user guide.

Compile ESMF
============

Install step 3.3: Compile ESMF (current working directory: $HOME/scripps_kaust_model/esmf)::

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
