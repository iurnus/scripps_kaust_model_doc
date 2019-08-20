############################
Install the model components
############################

The coupled model can be downloaded by using::

  git clone https://github.com/iurnus/scripps_kaust_model.git

It can be also downloaded from Github directly. This coupled model is maintained on Github and this
documentation is for SCRIPS v1.0. 

To install SKRIPS, the oceanic component MITgcm, the atmospheric component WRF, and the coupler ESMF
should be installed first. I would recommend installing the components in the same folder as SCRIPS.

There are two makefiles in the GIT repository. If you are using ring@ucsd.edu (PGI compiler), I
would recommend you use::

  ./Allmake.ring.sh

If you are using Shaheen-II at KAUST (Cray XC CLE/Linux x86_64, Xeon ifort compiler), I would
recommend you use::

  ./Allmake.shaheen.sh

I use *ring* and *shaheen* to compile and test the coupled model. If you are not using ring or
shaheen, please follow the detailed instruction below:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
