############
Install ESMF
############

Download ESMF
=============

ESMF 8.6.0 is available at:

    https://github.com/esmf-org/esmf/releases/tag/v8.6.0

The earlier releases of ESMF can be found at:

    http://earthsystemmodeling.org/download/

Install ESMF on kala (with GNU compiler)
========================================

Install step 2.1: Download ESMF 8.6.0::

  cd $SKRIPS_DIR
  wget https://github.com/esmf-org/esmf/archive/refs/tags/v8.6.0.zip
  unzip v8.6.0.zip
  mv esmf-8.6.0 esmf

Examine the bashrc file
=======================

Install step 2.2: In the ``~/.bashrc_skrips`` file, we use the following ESMF
configurations::

  # ESMF compile options
  export ESMF_OS=Linux
  export ESMF_COMM=openmpi
  export ESMF_NETCDF=nc-config
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_COMPILER=gfortran
  export ESMF_SITE=default
  export ESMF_PIO=internal

  export ESMF_LIB=$ESMF_DIR/lib/lib$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMF_MOD=$ESMF_DIR/mod/mod$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk
  export ESMF_TESTEXHAUSTIVE=ON
  export ESMF_TESTMPMD=OFF
  export ESMF_TESTHARNESS_ARRAY=RUN_ESMF_TestHarnessArray_default
  export ESMF_TESTHARNESS_FIELD=RUN_ESMF_TestHarnessField_default
  export ESMF_TESTWITHTHREADS=OFF

To install ESMF on a different machine, one must check if these options are in
conflict with the system environment:

.. note::

  #. *ESMF_COMPILER=gfortran* means I am using gfortran compiler. 
  #. *ESMF_COMM=openmpi* means I am using OpenMPI. 
  #. *ESMF_TEST\** options are used for testing the compiled ESMF libraries.
  #. The explaination of other configurations is documented in ESMF user guide.

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
also compile the coupled code when a few unit tests are failed. On ESMF
official website, some unit tests could also fail on specific machines.
Currently we don't know which specific tests must pass for the coupled code.

More summaries of ESMF releases are available here: 
    https://earthsystemmodeling.org/release/platforms_8_6_0
    https://earthsystemmodeling.org/static/releases.html
