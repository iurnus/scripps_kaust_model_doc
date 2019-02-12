############
Install ESMF
############

Download ESMF
=============

ESMF is available at:

    https://www.earthsystemcog.org/projects/esmf/download/

The earlier versions of ESMF can be found at:

    http://www.earthsystemmodeling.org/download/data/releases.shtml

Install ESMF7.0.0 Using PGI, OpenMPI, and NetCDF4
=================================================

First, we need to compile OpenMPI and NetCDF4 to use ESMF v7.0.0. 

Second, check the files in installOption_OTH/esmfInstallOptions::

    build_rules.mk.ring
    configure.esmf.netcdf3
    configure.esmf.netcdf4
    configure.esmf.ring
    configure.esmf.tscc
    configure.esmf.shaheen
    README.ring
    README.tscc
    version.pgCC.ring

Copy configure.esmf.netcdf4 to the ESMF folder. Then source the configuration
file::

    cd configurations.esmf.ring $ESMF_DIR/configurations.esmf
    . configure.esmf

Finally, change the following files ($ESMF_DIR is the ESMF directory):

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

Using the default options
-------------------------

Required Libraries: no libraries are required
Code directory: /home/rus043/kaust_project/esmf/esmf/
Process:

Set the DIR of the ESMF files::

    export ESMF_DIR=/home/rus043/kaust_project/esmf/esmf/

Check the information of necessary configurations::

    gmake info

Compile the code::

    gmake
 
Using NetCDF3
-------------

Since the installation of NetCDF3 is slightly different than NetCDF4, the following steps are
required to activate NetCDF3 (on ring@ucsd)::

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

Some known compilation errors 
=============================

(1) /usr/bin/ld: /project_shared/Libraries/netcdf-3.6.3_pgi_fortran_17.4-0//lib/libnetcdf.a(attr.o):
relocation R_X86_64_32S against '.data' can not be used when making a shared object; recompile with
-fPIC
/project_shared/Libraries/netcdf-3.6.3_pgi_fortran_17.4-0//lib/libnetcdf.a: error adding symbols:
Bad value

The NetCDF library is not compiled using shared library. You need to re-install NetCDF, compile it
with -fPIC, and check if libnetcdf.so file is generated.

(2) /usr/bin/ld: cannot find -lmpi_cxx

The mpi_cxx library is not available now in the current OpenMPI version. Use -lmpi instead in the
build_rule.mk file. Also, the following command can be used to check which library mlicxx depends
on::

    mpicxx -showme:libs

(3) /usr/bin/ld: ESMCI_StringSubr.o: undefined reference to symbol
'_ZNKSt5ctypeIcE13_M_widen_initEv@@GLIBCXX_3.4.11'

It seems that the location of library that ESMC_StringSubr.o depend on is not known to the system.
Check the library by using::

    strings /usr/lib64/libstdc++.so.6 | grep _ZNKSt5ctypeIcE13_M_widen_initEv_ZNKSt5ctypeIcE13_M_widen_initEv

And, add /usr/lib64/ to the LD_LIBRARY_PATH variable in the ~/.bashrc

To see what a Library is linked to::

    [cpapadop@ring lib64]$ ldd libstdc++.so.6
    linux-vdso.so.1 =>  (0x00007ffd7bdf8000)
    libm.so.6 => /lib64/libm.so.6 (0x00007f569cdd9000)
    libc.so.6 => /lib64/libc.so.6 (0x00007f569ca17000)
    /lib64/ld-linux-x86-64.so.2 (0x00007f569d3f9000)
    libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007f569c801000)

(4) /home/rus043/kaust_project/esmf/esmf_pgi//lib/libO/Linux.pgi.64.openmpi.default/libesmf.so:
undefined reference to 'netcdf_nf90_put_var_4d_eightbytereal\_'

The undefined reference issue is because the the library is not added when compiling the unit test
programs. The solution if to add “-netcdff” to the end of the compling command.

(5) ESMF compiler output from different systems (can be used as the compiler reference): 
https://www.earthsystemcog.org/projects/esmf/platforms_7_0_0
