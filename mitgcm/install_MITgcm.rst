##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

The MITgcm code is available at:

http://mitgcm.org/public/source_code.html

After downloading the code (we are using c66h version of MITgcm, other versions
should be fine), open the file in *$HOME/scripps_kaust_model/*::

    tar -xf MITgcm_c66h.tar.gz

Enter the folder::

    cd $HOME/scripps_kaust_model/MITgcm_c66h/

Compile the code
----------------

Below are the steps to compile the MITgcm test cases.

Compile the code using default setup
====================================

To compile the code using the default setup is straightforward with only a few steps. First, you
need to *cd* to one of the cases, for example::

    cd ./verification/tutorial_barotropic_gyre/

where $MITGCM_DIR is the location of the MITgcm folder.

Then, run the following commands to compile using GNU without MPI (default gfortran compiler)::

    cd build
    ../../../tools/genmake2 "-mods" "../code" 
    make depend 
    make

After the code is successfully built, an executable file *mitgcmuv* can be seen in the *build*
folder. Using the default compiler is very simple, but sometimes the user would need to add more
options when compiling the code.

