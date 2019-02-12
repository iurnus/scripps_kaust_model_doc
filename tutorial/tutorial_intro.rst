.. _tutorial_intro:

##################################
Introduction of the tutorial cases
##################################

Introduction
============

The test tutorial cases are prepared with different levels of complexities. For
example, L1.C1 means the complexity level is 1 and the case index is 1. L3.C2
means the complexity level is 3 and the case index is 2. 

Level 1 cases
-------------
The simulation cases are classified using different level of complexities. In
level 1, only the cases for the stand-alone cases are prepared. These cases can
be used to test if the stand-alone solvers are compiled correctly. 

Level 1, stand-alone cases:

L1.C1.mitgcm_case_CA2009

* MITgcm stand-alone case
* California Region
* start day: 2009 01 01

L1.C2.coupled_esmf_test

* ESMF stand-alone case
* test the coupled ocean--atmosphere prototype solver


Level 2 cases
-------------
In level 2, some test cases for the ESMF interfaces are added, including
ESMF--MITgcm and ESMF--WRF.

Level 2, ESMF--X interface test cases:

L2.C1.mitgcm_case_CA2009_mpiESMF_1cpu

* MITgcm part same as L1.C1 case
* Use ESMF--MITgcm interface
* Using 1 CPU, MPI not activated

L2.C2.mitgcm_case_CA2009_mpiESMF_2cpu

* MITgcm part same as L1.C1 case
* Use WRF--MITgcm interface
* Using 2 CPU, MPI activated

Level 3 cases
-------------
In level 3, the test cases for the coupled solver are added. The test cases
include the California region test and the Red Sea region test. We also added
the coupled solver that is able to run on TSCC machines using intel compiler.

Level 3, CPL run cases:

L3.C1.coupled_RS2012_ring

* Heat wave event starts from 2012 06 01
* MITgcm part use Red Sea
* WRF part use Red Sea
* Use ESMF--MITgcm interface
* Use ESMF--WRF interface
* MPI activated
* Use WRF bulk formula

Post-processing
---------------
The post-processing scripts are also provided in the \script-post folder.

