####################
Install ESMF on ring
####################

Download ESMF
=============

ESMF is available at:

    https://www.earthsystemcog.org/projects/esmf/download/

The earlier versions of ESMF can be found at:

    http://www.earthsystemmodeling.org/download/data/releases.shtml

Install ESMF7.0.0 Using PGI, OpenMPI, and NetCDF4
=================================================

First, we need to compile OpenMPI and NetCDF4 to use ESMF v7.0.0. 

Second, check the files in *installOption_OTH/esmfInstallOptions*::

    build_rules.mk.ring
    configure.esmf.netcdf3
    configure.esmf.netcdf4
    configure.esmf.ring
    configure.esmf.tscc
    configure.esmf.shaheen
    README.ring
    README.tscc
    version.pgCC.ring

Copy *configure.esmf.ring* to the ESMF folder. Then source the configuration
file::

    cd configurations.esmf.ring $ESMF_DIR/configurations.esmf
    . configure.esmf

Finally, I need to change the following files because ring@ucsd.edu is using PGI 17. They might be
different when using different versions of PGI compiler.

(1) $ESMF_DIR/build_config/Linux.pgi.default/build_rules.mk

before::

    ESMF_CXXDEFAULT         = pgCC
    ESMF_F90LINKLIBS       += -lmpi_cxx
    ESMF_F90LINKLIBS += -pgcpplibs -ldl

after::

    ESMF_CXXDEFAULT         = pgc++
    ESMF_F90LINKLIBS       += -lmpi
    ESMF_F90LINKLIBS += -pgc++libs -ldl

(2) $ESMF_DIR/scripts/version.pgCC (not very sure about this)

before::

    GCCEXE=`echo $PGCCEXE | sed 's/\.pg.*TermX/pgCC/g’`

after::

    GCCEXE=`echo $PGCCEXE | sed 's/\.pg.*TermX/pgc++/g’`


Check the information of necessary configurations::

    gmake info

Compile the code::

    gmake
 
If it is the first time ESMF is installed, make sure to test ESMF using::

    gmake all_tests

If ESMF7.0.0 is successfully built, the unit tests will pass.

(test results on ring: NetCDF4: 7075/7084 tests pass; for NetCDF3, 7084/7084)

The perfect build summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_7_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/700/700_PC-Xeon-Cluster_Discover_PGI.html


Use other options
=================

Using the default options on ring
---------------------------------

Required Libraries: no libraries are required
Code directory: /home/rus043/kaust_project/esmf/esmf/
Process:

Set the DIR of the ESMF files::

    export ESMF_DIR=/home/rus043/kaust_project/esmf/esmf/

Check the information of necessary configurations::

    gmake info

Compile the code::

    gmake
 
Using NetCDF3 on ring
---------------------

Since the installation of NetCDF3 is slightly different than NetCDF4, the following steps are
required to activate NetCDF3 on ring@ucsd::

    1 
    2 MPI_HOME="/project_shared/Libraries/openmpi-2.1.1_pgi_fortran_17.5-0/include"
    3 
    4 export ESMF_DIR=$(pwd)
    5 export ESMF_OS=Linux
    6 export ESMF_COMM=openmpi
    7 export ESMF_NETCDF=split
    8 export NETCDF=/project_shared/Libraries/netcdf-3.6.3_shared_pgi_fortran_17.5-0/
    9 export ESMF_NETCDF_INCLUDE=${NETCDF}include
    10 export ESMF_NETCDF_LIBPATH=${NETCDF}lib
    11 export ESMFMKFILE=${ESMF_DIR}lib/libg/Linux.pgi.64.openmpi.default/esmf.mk
    12 
    13 export ESMF_OPENMP=OFF
    14 export ESMF_TESTMPMD=OFF
    15 export ESMF_TESTHARNESS_ARRAY=RUN_ESMF_TestHarnessArray_default
    16 export ESMF_TESTHARNESS_FIELD=RUN_ESMF_TestHarnessField_default
    17 export ESMF_TESTWITHTHREADS=OFF
    18 export ESMF_LAPACK=internal
    19 export ESMF_TESTEXHAUSTIVE=ON
    20 export ESMF_BOPT=g
    21 export ESMF_SITE=default
    22 export ESMF_ABI=64
    23 export ESMF_COMPILER=pgi

