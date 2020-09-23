##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

Install step 6: Download MITGCM (current working directory: $HOME/scripps_kaust_model-1.1/esmf)::

  cd ..
  wget https://github.com/MITgcm/MITgcm/archive/checkpoint67m.zip
  unzip checkpoint67m.zip
  mv MITgcm-checkpoint67m MITgcm_c67m

Compile the code
----------------

Below are the steps to compile MITgcm on ring.ucsd.edu

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
folder. 

Compile the code using PGI and OpenMPI
======================================

To compile MITgcm using PGI and OpenMPI, the following commands are required (the option file is
written by Caroline in the shared folder)::

    export MPI_HOME="/project_shared/Libraries/openmpi-2.1.1_pgi_fortran_17.5-0/include"
    cd $HOME/scripps_kaust_model-1.1/verification/global_ocean.cs32x15/build/
    ../../../tools/genmake2 "-mpi" "-mods" "../code" "-optfile" "/home/cpapadop/MITGCM_WRF/sio_build_options/ring_build_pgi_17.5-0_openmpi_2.1.1_netcdf.3.6.3"
    make depend
    make -j8

The *make -j8* command can compile the code in parallel using 8 processors. The same executable file
*mitgcmuv* can be seen in the *build* folder.
