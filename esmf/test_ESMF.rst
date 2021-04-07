#########
Test ESMF
#########

Test ESMF with applications
===========================

An ESMF application can better test ESMF.

Case 1: ESMF coupled flow demonstration case
============================================

The demonstration cases can be found at:

https://www.earthsystemcog.org/projects/esmf/external_demos

The tutorial of the demonstration case can be found at:

(a) PDF: https://www.earthsystemcog.org/site_media/projects/esmf/ESMF_CoupledFlow.pdf

(b) html: http://www.earthsystemmodeling.org/users/training/tutorials_noheader/index.html

Compile the demonstration case
------------------------------

The demonstration case in our git repository is slightly different from the original one. We have
fixed the bug in the test case, modified the parameters in the simulations, and added a simple
post-processing MATLAB code. 

Open the demonstration case (current working directory: $HOME/scripps_kaust_model/esmf)::

    cd ../esmf_test_application/esmf_test_coupled_flow
    # compile the case::
    make

Run the demonstration case
--------------------------

To run the demonstration case in parallel::

    mpirun -np 4 ESMF_CoupledFlow

The demonstration case generates a series of output files::

    PET0.ESMF_LogFile - ESMF log file containing all error messages logged.
    DE.nc             - NetCDF file containing decomposition element ids.
    FLAG.nc           - NetCDF file containing flag of boundary conditions.
    OMEGA.nc          - NetCDF file containing time series of vorticity.
    SIE.nc            - NetCDF file containing time series of internal energy.
    U_velocity.nc     - NetCDF file containing time series of U velocity component.
    V_velocity.nc     - NetCDF file containing time series of V velocity component.

* All output data files are removed by the "make dust".
* "make clean" removes all object, module and the executable file.
* "make distclean" combines the "dust" and "clean" targets.


Notes
-----

Compared to the original version, the following lines in CouplerMod.F90 file are commented out::

    253            ! check isneeded flag here
    254            !! if (.not. isFieldNeeded(importState, datanames(i), rc=rc)) then 
    255            !!     !print *, "skipping field ", trim(datanames(i)), " not needed"
    256            !!     cycle
    257            !! endif


Case 2: NUOPC prototype code
============================

There are several NUOPC prototype code examples:

https://sourceforge.net/p/esmfcontrib/svn/HEAD/tree/NUOPC/
http://www.earthsystemmodeling.org/esmf_releases/non_public/ESMF_8_0_0/NUOPC_howtodoc/

This is an example of ATM--CON--OCN model based on NUOPC:

https://sourceforge.net/p/esmfcontrib/svn/HEAD/tree/NUOPC/tags/ESMF_8_0_0/AtmOcnConProto/

We have also added this example case to our project (current working directory:
$HOME/scripps_kaust_model/esmf_test_application/esmf_test_coupled_flow). To compile and run this
example:: 

    cd ../AtmOcnConProto/
    make
    mpirun -np 6 esmApp
