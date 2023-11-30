.. _test_cpl_ww3:

#######################################
Run the coupled case with WaveWatch III
#######################################

Case initialization
===================

To install the coupled case with WaveWatch III, enter the L4C1 case folder ::

  cd $SKRIPS_DIR/coupler/L4.C1.coupled_RS2012_ring/
  ./install.sh

Initialize the test case ::

  cd runCase.init
  ./Allrun

Run the coupled case
====================

Enter the *runCase* folder and run the test case::

  [ruisun@acc00]~/.../coupler/L4.C1.coupled_RS2012_ring/runCase.init$ cd ../runCase/
  [ruisun@acc00]~/.../coupler/L4.C1.coupled_RS2012_ring/runCase$ ./Allrun

Similar with the L3C1 case, the *namelist.rc* file controls the coupled run::

  DebugLevel:     0
   
  ## mode 1 = sequential mode
  ## mode 2 = concurrent mode
  coupleMode:     1
  cpuOCN:        16
  cpuATM:        16
  cpuWAV:        16

  ## Set NX and NY for WaveWatch III in ESMF grid 
  ## NX * NY = cpuWAV 
  waveNPX:        4
  waveNPY:        4
  
  StartYear:      2012
  StartMonth:     06
  StartDay:       01
  StartHour:      00
  StartMinute:    00
  StartSecond:    00
 
  StopYear:        2012
  StopMonth:       06
  StopDay:         01
  StopHour:        03
  StopMinute:      00
  StopSecond:      00
  
  ## EsmStepSeconds is the coupling time step
  EsmStepSeconds: 600
  ## ATMStepSeconds is the time step for WRF
  ATMStepSeconds: 60
  ## OCNStepSeconds is the time step for MITgcm
  OCNStepSeconds: 600
  ## The wave model is not activated
  WAVStepSeconds: 600

In namelist.input file, I added the following lines to control the input and output from ESMF::

  ## auxinput5_interval_s sets the coupling interval for ESMF inputs
  30  auxinput5_inname                    = 'wrfin_esmf',
  31  auxinput5_interval_s                = 60,
  32  auxinput5_end_d                     = 60,
  33  io_form_auxinput5                   = 7,
  ## auxhist5_interval_s sets the coupling interval for ESMF outputs
  34  auxhist5_outname                    = 'wrfout_esmf',
  35  auxhist5_interval_s                 = 60,
  36  auxhist5_end_d                      = 60,
  37  io_form_auxhist5                    = 7,
  
In namelist.input file, I added the following lines to control the coupling schemes for WRF::

  ## Option 6: Chanock;
  ## Option 7: Taylor and Yelland;
  ## Option 8: OOST;
  86  isftcflx                            = 7,
  ## Option 0: use COARE 3.0 scheme;
  ## Option 1: use COARE 3.5 scheme;
  87  use_coare35                         = 0,
  
In data file, I added the following lines to control coupling scheme for MITgcm::

  44 # Options when coupled with WW3
  ## Option 0: use wind stress from MITgcm;
  ## Option 1: use wind stress from WW3;
  41  stressFromWave = 0,
  ## Option 0: do not activate Langmuir turbulence in KPP;
  ## Option 1: use LF16-MA;
  ## Option 2: use LF16-EN;
  ## Option 3: use LF17;
  ## Option 5: use LF17, but with parameterized wave;
  42  langmuirScheme = 0,
  ## Option 0: do not activate the Stokes-Coriolis or Stokes advection;
  ## Option 1: use Stokes-Coriolis and Stokes advection;
  ## To activate Stokes advection, MITgcm MUST have multiDimAdvection = .TRUE.
  ## To activate Stokes-Coriolis, MITgcm MUST have vectorInvariantMomentum = .FALSE.
  ## stokesProfile = 0: no stokes profile (turn off stokesCoriolis and stokesAdvection)
  ## stokesProfile = 1: use Eq. (7) in Breivik 2014 paper (https://doi.org/10.1175/JPO-D-14-0020.1)
  ## stokesProfile = 2: use Eq. (16) in Breivik 2014 paper
  43  stokesProfile = 0,
  44  stokesCoriolis = 0,
  45  stokesAdvection = 0,



