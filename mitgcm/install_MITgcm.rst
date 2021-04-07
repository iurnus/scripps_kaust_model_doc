##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

Install step 6: Download MITGCM (current working directory: $HOME/scripps_kaust_model/esmf)::

  cd ..
  wget https://github.com/MITgcm/MITgcm/archive/checkpoint67m.zip
  unzip checkpoint67m.zip
  mv MITgcm-checkpoint67m MITgcm_c67m

Compile the code (using default setup)
--------------------------------------

To compile the code using the default setup is straightforward::

    cd MITgcm_c67m/verification/tutorial_barotropic_gyre/

Then, run the following commands to compile using default compiler without MPI::

    cd build
    ../../../tools/genmake2 "-mods" "../code" 
    make depend 
    make

After the code is successfully built, an executable file *mitgcmuv* can be seen in the *build*
folder.

