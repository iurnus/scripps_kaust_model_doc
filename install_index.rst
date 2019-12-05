############################
Install the model components
############################

The official repository of SKRIPS 1.1 is maintained on Github::

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v1.1

Install step 1: Download this version from Github::

  wget https://github.com/iurnus/scripps_kaust_model/archive/v1.1.zip
  unzip v1.1.zip
  cd scripps_kaust_model-1.1
  ls -l

You will see the following folders and files::

  -rwxrwxr-x  1 ruisun ruisun  132 2019-11-26 17:18 Allclean.sh
  -rwxrwxr-x  1 ruisun ruisun 4150 2019-11-26 17:18 Allmake.ring.sh
  -rwxrwxr-x  1 ruisun ruisun 4108 2019-11-26 17:18 Allmake.shaheen.sh
  drwxrwxr-x 10 ruisun ruisun 4096 2019-11-26 17:18 coupler
  drwxrwxr-x  4 ruisun ruisun   59 2019-11-26 17:18 esmf_test_application
  drwxrwxr-x  5 ruisun ruisun  140 2019-11-26 17:18 installOption_OTH
  drwxrwxr-x  5 ruisun ruisun  149 2019-11-26 17:18 installOption_WRF
  drwxrwxr-x  2 ruisun ruisun   52 2019-11-26 17:18 license_statements
  -rw-rw-r--  1 ruisun ruisun 2946 2019-11-26 17:18 README.md

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
