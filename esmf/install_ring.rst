####################
Install ESMF on ring
####################

Install step 3: Load PGI compiler, OpenMPI and NetCDF (current working directory:
$HOME/scripps_kaust_model/esmf)::

    . ../installOption_OTH/bash_setup_ring

Install step 4: Set ESMF configurations (current working directory: 
$HOME/scripps_kaust_model/esmf)::

    . ../installOption_OTH/configurations.esmf.ring

Install step 5: Compile ESMF (current working directory:
$HOME/scripps_kaust_model/esmf)::

    # Check the information of necessary configurations
    gmake info &> log.info

    # Compile the code
    gmake &> log.gmake

    # If it is the first time ESMF is installed, make sure to test ESMF using::
    gmake all_tests &> log.all_tests
