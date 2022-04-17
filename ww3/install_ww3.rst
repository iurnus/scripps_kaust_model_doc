.. _install_ww3:

###########
Install WW3
###########

Update WW3 configurations
=========================

Install step 5.1: Download WW3 v6.07.1::

  cd $SKRIPS_DIR
  wget https://github.com/NOAA-EMC/WW3/archive/refs/tags/6.07.1.zip
  unzip 6.07.1.zip
  mv WW3-6.07.1 ww3_607
  # save a copy
  cp -rf ww3_607 ww3_607.org


Install step 5.2: Set the auxiliary FORTRAN and C compiler::
  
  cd ww3_607
  w3_setup $WW3_DIR/model -c intel
    
Use ifort and cc for Shaheen::
  
  Auxiliary FORTRAN compiler [gfortran] : ifort
  Auxiliary C compiler [gcc] : cc

Use mpif90 and mpicc for COMET::
  
  Auxiliary FORTRAN compiler [gfortran] : mpif90
  Auxiliary C compiler [gcc] : mpicc

Install step 5.3: Compile WaveWatch III::

  cd $SKRIPS_DIR
  cd installOption_WW3
  ./install_ww3_comet.sh
  
Notes
=====

1. When using Shaheen, I would use ifort and cc as the auxiliary compilers.

2. When using ring, Step 5.2 I would use "w3_setup $WW3_DIR/model"
