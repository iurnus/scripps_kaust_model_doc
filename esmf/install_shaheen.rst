##########################
Install ESMF on Shaheen-II
##########################

Install ESMF7.0.0 on Shaheen-II
===============================

First, we need to load the necessary modules on Shaheen-II (intel compiler)::

  . ./installOption_OTH/bash_setup_shaheen
    
Second, copy *configure.esmf.shaheen* to the ESMF folder. Then source the configuration
file::

    cd configurations.esmf.shaheen $ESMF_DIR/configurations.esmf
    . configure.esmf

Check the information of necessary configurations::

    gmake info

Compile the code::

    gmake
 
The perfect build summary on the ESMF website is: 
https://www.earthsystemcog.org/projects/esmf/platforms_7_0_0
http://www.earthsystemmodeling.org/download/platforms/reports/700/700_PC-Xeon-Cluster_Discover_PGI.html

