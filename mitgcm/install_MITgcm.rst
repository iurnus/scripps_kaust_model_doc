##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

Install step 6: Download MITGCM (current folder: $HOME/scripps_kaust_model-1.1/esmf)::

  cd ..
  wget https://github.com/MITgcm/MITgcm/archive/checkpoint67m.zip
  unzip checkpoint67m.zip
  mv MITgcm-checkpoint67m MITgcm_c67m

Compile the code
----------------

Below are the steps to compile the MITgcm test cases.

Compile the code using default setup
====================================

To compile the code using the default setup is straightforward with only a few steps. First, you
need to *cd* to one of the cases, for example::

    cd MITgcm_c67m/verification/tutorial_barotropic_gyre/

where $MITGCM_DIR is the location of the MITgcm folder.

Then, run the following commands to compile using GNU without MPI (default gfortran compiler)::

    cd build
    ../../../tools/genmake2 "-mods" "../code" 
    make depend 
    make

After the code is successfully built, an executable file *mitgcmuv* can be seen in the *build*
folder. Using the default compiler is very simple, but sometimes the user would need to add more
options when compiling the code.

