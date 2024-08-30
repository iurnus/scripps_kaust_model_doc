##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

Install step 3: Download MITGCM::

  cd $SKRIPS_DIR
  wget https://github.com/MITgcm/MITgcm/archive/checkpoint68r.zip
  unzip checkpoint68r.zip
  mv MITgcm-checkpoint68r MITgcm_c68r

Compile the code (using default setup)
--------------------------------------

To compile the code using the default setup is straightforward::

    cd MITgcm_c68r/verification/tutorial_barotropic_gyre/

Then, run the following commands to compile using default compiler without MPI::

    cd build
    ../../../tools/genmake2 "-mods" "../code" 
    make depend 
    make

After the code is successfully built, an executable file *mitgcmuv* can be seen in the *build*
folder.

