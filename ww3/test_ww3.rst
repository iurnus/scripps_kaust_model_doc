.. _test_ww3:

########
Test WW3
########

Update WW3 configurations
=========================

Install step ?: Download WW3 v6.07.1 (current working directory:
$HOME/scripps_kaust_model/)::

  cd ..
  wget https://github.com/NOAA-EMC/WW3/archive/refs/tags/6.07.1.zip
  unzip 6.07.1.zip
  mv WRF-4.1.3 WRFV413_AO
  # save a copy
  cp -rf WRFV413_AO WRFV413_AO.org
