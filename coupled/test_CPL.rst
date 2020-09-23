.. _test_cpl:

####################
Run the coupled case
####################

Case initialization
===================

To test the coupled case, enter *runCase.init* folder (current working directory:
$HOME/scripps_kaust_model-1.1/couplers/L3.C1.coupled_RS2012_ring/)::

  [ruisun@acc00]~/scripps_kaust_model_test/coupler/L3.C1.coupled_RS2012_ring$ cd runCase.init/
  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ ls
  Allclean  Allrun  namelist.input  README


Initialize the test case::

  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ ./Allrun

Run the coupled case
====================

Enter the *runCase* folder and run the test case::

  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase.init$ cd ../runCase/
  [ruisun@acc00]~/.../coupler/L3.C1.coupled_RS2012_ring/runCase$ ./Allrun

In the Allrun file, MATLAB is required for pre-processing (the python scripts
are available in the L3.C3 case for Shaheen-II at KAUST)::

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

The *namelist.rc* file controls the coupled run::

  Debuglevel: 0
  ## mode 1 = sequential mode
  ## mode 2 = concurrent mode
  coupleMode: 1
  cpuOCN:     4
  cpuATM:     4
  
  StartYear:  2012
  StartMonth:   06
  StartDay:     01
  StartHour:    00
  StartMinute:  00
  StartSecond:  00
  
  EndYear:    2012
  EndMonth:     06
  EndDay:       01
  EndHour:      06
  EndMinute:    00
  EndSecond:    00
  
  ## EsmStepSeconds is the coupling time step
  EsmStepSeconds: 60
  ## ATMStepSeconds is the time step for WRF
  ATMStepSeconds: 60
  ## OCNStepSeconds is the time step for MITgcm
  OCNStepSeconds: 60
  ## The wave model has not implemented yet
  WAVStepSeconds: 60
