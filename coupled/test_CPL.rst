.. _test_cpl:

#################
Test coupled code
#################

How to test
===========

There are a few test cases.

To test the coupled case, enter *runCase.init* folder (current folder *$HOME/scripps_kaust_model/couplers/L3.C1.coupled_RS2012_ring/*)::

  [ruisun@acc00]~/scripps_kaust_model_test/coupler/L3.C1.coupled_RS2012_ring$ cd runCase.init/
  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ ls
  Allclean  Allrun  namelist.input  README


Initialize the test case::

  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ ./Allrun

Enter the *runCase* folder and run the test case::

  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ cd ../runCase/
  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase$ ./Allrun

In the Allrun file, MATLAB is required for pre-processing (The python scripts
are available in the L3.C3 case for shaheen@kaust)::

  \3 ln -sf ../caseInput/* .
  \4 
  \5 cp ../../../WRFV3911_AO/main/wrf.exe .
  \6 cp ../runCase.init/wrfbdy_d01 . 
  \7 cp ../runCase.init/wrfinput_d01 .
  \8 cp ../runCase.init/wrflowinp_d01 .
  \9 matlab -nodisplay < updateLowinp.m
  10 
  11 cp namelist.input.set namelist.input
  12 mpirun -np 4 wrf.exe
  13 matlab -nodisplay < updateHFlux.m
  14 
  15 cp namelist.input.run namelist.input
  16 cp ../coupledCode/esmf_application .
  17 echo "running coupled MITgcm--WRF simulation.."
  18 mpirun -np 4 esmf_application &> log.esmf



