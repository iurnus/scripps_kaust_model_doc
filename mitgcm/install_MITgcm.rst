##############
Install MITgcm
##############

Get the code and documentations
-------------------------------

Install step 3: Download MITgcm::

  cd $SKRIPS_DIR
  wget https://github.com/MITgcm/MITgcm/archive/checkpoint68r.zip
  unzip checkpoint68r.zip
  mv MITgcm-checkpoint68r MITgcm_c68r

MITgcm tutorial case
--------------------

The MITgcm user guide is available here:

   https://mitgcm.readthedocs.io/en/latest/index.html

To compile and run MITgcm, please refer to Chapter 3 and 4:

   https://mitgcm.readthedocs.io/en/latest/getting_started/getting_started.html

   https://mitgcm.readthedocs.io/en/latest/examples/examples.html

For example, for the MITgcm barotropic gyre case, you can try the following
commands to compile it without MPI::
 
     cd $SKRIPS_DIR/MITgcm_c68r/verification/tutorial_barotropic_gyre/
     cd build
     ../../../tools/genmake2 "-mods" "../code" 
     make depend 
     make

After the code is successfully built, an executable file ``mitgcmuv`` can be
seen in the ``build`` folder.

