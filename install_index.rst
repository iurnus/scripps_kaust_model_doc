############################
Install the model components
############################

The official repository of SKRIPS 1.1 is::

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v1.1

This coupled model is maintained on Github and this documentation is for SKRIPS v1.1. 

To install SKRIPS, the oceanic component MITgcm, the atmospheric component WRF, and the coupler ESMF
should be installed first. I would recommend installing the components in the same folder as SKRIPS.

I have tested SKRIPS on two computers and saved two makefiles. For ring@ucsd.edu (PGI compiler), I
would use::

  ./Allmake.ring.sh

For Shaheen-II at KAUST (Cray XC CLE/Linux x86_64, Xeon ifort compiler), I would use::

  ./Allmake.shaheen.sh

If you are not using ring@ucsd.edu or Shaheen-II, the two makefiles would not work. Please follow
the detailed instruction below:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
   Install and test coupled code<coupled/index>
