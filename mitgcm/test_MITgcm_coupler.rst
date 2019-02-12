#############################
MITgcm test case in CA region
#############################

MITgcm example case
-------------------

Compile the code
~~~~~~~~~~~~~~~~

We have an example MITgcm case in our coupled code::

    cd $PROJECT_DIR/coupler/L1.C1.mitgcm_case_CA2009

We have the following files in the test case::

    clean.sh
    input_ca_mitgcm\
    install.sh
    mitCode\
    mitRun\
    README
    utils\

To compile and run this case::
    ./install.sh


MITgcm--ESMF coupled case
-------------------------

The L2C1 case is to test the coupled interface between MITgcm and ESMF::

    ll $PROJECT_DIR/coupler/L2.C1.mitgcm_case_CA2009_mpiESMF_1cpu

To compile and run this case::
    ./install.sh

The results obtained from the L2C1 case should be the same as the L1C1 case.
