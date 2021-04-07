##########################
Install ESMF on Shaheen-II
##########################

Install step 3: Load PGI compiler, OpenMPI and NetCDF (current working directory:
$HOME/scripps_kaust_model/esmf)::

    . ../installOption_OTH/bash_setup_shaheen

Install step 4: Set ESMF configurations (current working directory:
$HOME/scripps_kaust_model/esmf)::

    . ../installOption_OTH/configurations.esmf.shaheen

Install step 5: Compile ESMF (current working directory:
$HOME/scripps_kaust_model/esmf)::

    # Check the information of necessary configurations
    gmake info &> log.info

    # Compile the code
    gmake &> log.gmake

    # the tests cannot run successfully on Shaheen because of their unique system
    # gmake all_tests &> log.all_tests
