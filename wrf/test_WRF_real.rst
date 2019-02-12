.. _test_wrf_real:

############################
Test WRF using the real case
############################

Install WRF for the real case
==============================

First, we install the WRF for real case simulations according to :ref:`WRF install guide<install_wrf>`.

Run WRF
=======

For the real California region case, change to the
*$PROJECT_DIR/coupler/L1.C2.wrf_case_CA2009/* folder and execute the *install.sh*
script::

    ./install.sh

If we take a look at the *install.sh* script::

    1 #!/bin/csh -f
    2
    3 echo "running WRF..."
    4 cd wrfRunCA
    5 ./Allrun
    6 cd ..

An output file “wrfout_d01_2009-01-01_00:00:00” will be generated if no error occurs.

