############################
Install the model components
############################

The official repository of SKRIPS 1.2 is maintained on Github::

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v1.2


Install step 1: Download this version from Github::

  cd $HOME
  wget https://github.com/iurnus/scripps_kaust_model/archive/v1.2.zip
  unzip v1.2.zip
  mv scripps_kaust_model-1.2 scripps_kaust_model
  cd scripps_kaust_model
  ls -l

You will see the following folders and files (current working directory: $HOME/scripps_kaust_model)::

  drwxrwxr-x 10 ruisun ruisun 4096 2021-11-26 17:18 coupler
  drwxrwxr-x  4 ruisun ruisun   59 2021-11-26 17:18 esmf_test_application
  drwxrwxr-x  5 ruisun ruisun  140 2021-11-26 17:18 installOption_OTH
  drwxrwxr-x  5 ruisun ruisun  149 2021-11-26 17:18 installOption_WRF
  drwxrwxr-x  5 ruisun ruisun  149 2021-11-26 17:18 installOption_WW3
  drwxrwxr-x  2 ruisun ruisun   52 2021-11-26 17:18 license_statements
  -rw-rw-r--  1 ruisun ruisun 2946 2021-11-26 17:18 README.md

Please follow the detailed instruction below:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
   Install and test WW3<ww3/index>
   Install and test coupled code<coupled/index>
