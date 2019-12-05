####################
Install ESMF on ring
####################

Install step 3: Load PGI compiler, OpenMPI and NetCDF (now you are in folder
$HOME/scripps_kaust_model-1.1/esmf)::

    . ../installOption_OTH/bash_setup_ring

Install step 4: Set ESMF configurations (now you are in folder
$HOME/scripps_kaust_model-1.1/esmf)::

    cp ../installOption_OTH/esmfInstallOptions/configurations.esmf.ring .
    . configure.esmf.ring

Install step 5: Compile ESMF (now you are in folder
$HOME/scripps_kaust_model-1.1/esmf)::

    # Check the information of necessary configurations
    gmake info &> log.info

    # Compile the code
    gmake &> log.gmake

    # If it is the first time ESMF is installed, make sure to test ESMF using::
    gmake all_tests &> log.all_tests
