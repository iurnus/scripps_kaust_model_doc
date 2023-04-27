############
Install ESMF
############

Download ESMF
=============

ESMF 8.0.0 is available at:

    https://sourceforge.net/p/esmf/esmf/ci/ESMF_8_0_0/tree/

The earlier releases of ESMF can be found at:

    http://www.earthsystemmodeling.org/download/data/releases.shtml

Install ESMF Using PGI compiler
===============================

Install step 2.1: Download ESMF 8.0.0::

  cd $SKRIPS_DIR
  wget https://github.com/esmf-org/esmf/archive/refs/tags/ESMF_8_0_0.zip
  unzip ESMF_8_0_0.zip
  mv esmf-ESMF_8_0_0/ esmf


Examine the bashrc file
=======================

Install step 2.2: In the ~/.bashrc_skrips file, we have the ESMF configurations::

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
  export ESMF_MOD=$ESMF_DIR/mod/mod$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk

To install ESMF on a different machine or using different configurations, one
must update these ESMF options.

Several Notes::

1. *ESMF_COMPILER=intel* means I am using Intel compiler. 
2. *ESMF_COMM=intelmpi* means I am using Intel MPI. 
3. The explaination of other configurations is documented in ESMF user guide.

Compile ESMF
============

Install step 2.2: Compile ESMF::

    cd $SKRIPS_DIR/esmf

    # Check the information of necessary configurations
    gmake info &> log.info

    # Compile the code
    gmake &> log.gmake

    # If it is the first time ESMF is installed, make sure to test ESMF using::
    gmake all_tests &> log.all_tests

If ESMF is successfully built, all the unit tests should pass. However, we can
also compile the coupled code when a few unit tests failed. On ESMF official
website, some unit tests could also fail on specific machines. Currently we
don't know which specific tests must pass for the coupled code.

The complete summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_8_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/800/800_Discover_pgi-17.7.0.html
