.. _tutorial_rs:

###############################
Setting the red sea simulations
###############################

How to generate the MITgcm case for Red Sea
===========================================
1. generate the mesh
    1. Domain selection (E30-E50, N10-N30)
    2. Mesh size (256x256x40)

2. generate the bathymetry, based on ETOPO2 data
    1. Download ETOPO2 data (https://www.ngdc.noaa.gov/mgg/global/etopo2.html)
    2. Read bathymetry
    3. Search the entire region to give the initial MASK
    4. Run a simple MITgcm case to generate the MASK
    5. Generate the binary file for MITgcm run

3. generate the IC, based on HYCOM data
    1. get HYCOM IC data
    2. generate IC

4. generate the OBC, based on HYCOM data
    1. get HYCOM OBC data
    2. check the LAT/LON info
    3. generate OBCs without correction
    4. correct OBCs

5. (If necessary, generate the external forcing terms)

6. run MITgcm simulations

Run
===

The simulation of the Red Sea region starts from Jun 01 2012 to Jun 30 2012.
The ocean model MITgcm uses the heat fluxes, momentum fluxes and fresh water
fluxes from WRF, and WRF uses the Sea Surface Temperature from MITgcm.

To install and run the tutorial case for Red Sea, switch to
*coupler/L3.C1.coupled_RS2012_ring*::
  
    cd coupler/L3.C1.coupled_RS2012_ring/
    ./allrun.sh

The following executable is generated when the fully-coupled solver is
successfully compiled::

    coupledCode/esmf_application

The user can also execute the *install.sh* script to install the fully-coupled
solver.

Test and run the code
=====================

To run the coupled case for the Red Sea::

    cd runCase.init
    ./Allrun
    cd runCase
    ./Allrun

Clean the installation
======================
                                 
To clean the installed files::

    ./allclean.sh


Some known issues
=================

* In the eeboot.F file, I commented the “call eeboot_minimal” line to let ESMF
  to initialize the MPI library. (From the email of Dimitris, both eeboot and
  eeboot_minimal should be commented out to run the code. I have not tested
  which part of the code that generated this problem.)
* The post-processing of the result can be done using ncl or other tools
