############################
Install the model components
############################

To install the SKRIPS, the oceanic component MITgcm, the atmospheric component WRF, and the coupler
ESMF should be installed first. 

If you installed PGI compiler and compiled NETCDF using PGI, I would recommend you use::

  ./Allmake.ring.sh

If you installed Intel compiler and compiled NETCDF using Intel, I would recommend you use::

  ./Allmake.shaheen.sh

Here, *ring* and *shaheen* are two machines that I used to compile the coupled model. 

It is highly possible that other machines will not compile directly using these two makefiles. The
detail instruction on installing the model component are detailed:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
