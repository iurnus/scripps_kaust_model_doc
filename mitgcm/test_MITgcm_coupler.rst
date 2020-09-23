#############################
MITgcm test case in CA region
#############################

MITgcm example case
-------------------

Compile the code
~~~~~~~~~~~~~~~~

We have an example MITgcm case in our coupled code (current working directory:
$HOME/scripps_kaust_model-1.1/)::

    cd ./coupler/L1.C1.mitgcm_case_CA2009

Here, L1 denotes "Level 1 complexity"; C1 denotes "Case 1". 

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

The L2C1 case (Level 2 complexity, Case 1) is to test the interface between MITgcm and ESMF (current
working directory: $HOME/scripps_kaust_model-1.1/coupler/L1.C1.mitgcm_case_CA2009)::

    cd ../L2.C1.mitgcm_case_CA2009_mpiESMF_1cpu

To compile and run this case::
    ./install.sh

The results obtained from the L2C1 case should be the same as the L1C1 case. We also have L2C2 case
that uses more CPUs to test the MPI implementations.
