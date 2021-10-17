############################
Install the model components
############################

The official repository of SKRIPS v2.0 is maintained on Github::

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v2.0

Now I am using the SDSC supercomputer COMET as an example. To install the
SKRIPS model in the mead system
(/cw3e/mead/projects/csg102/rus043/scripps_kaust_model/), we have to download
the SKRIPS model and set up the bashrc file.

Install step 1.1: Download SKRIPS version from Github::

  # I am using my work folder as an example
  cd /cw3e/mead/projects/csg102/rus043/
  wget https://github.com/iurnus/scripps_kaust_model/archive/v2.0.zip
  unzip v2.0.zip
  mv scripps_kaust_model-2.0 scripps_kaust_model
  cd scripps_kaust_model
  ls -l

You will see the following folders and files::

  drwxr-sr-x  8 rus043 csg102 57344 Oct  4 10:56 coupler
  drwxr-sr-x  4 rus043 csg102 57344 Oct  4 10:56 esmf_test_application
  drwxr-sr-x  4 rus043 csg102 57344 Oct 11 14:05 installOption_OTH
  drwxr-sr-x  6 rus043 csg102 57344 Oct 11 09:12 installOption_WRF
  drwxr-sr-x  6 rus043 csg102 57344 Oct 10 23:39 installOption_WW3
  drwxr-sr-x  2 rus043 csg102 57344 Oct  4 10:56 license_statements
  -rw-r--r--  1 rus043 csg102  2597 Oct 11 11:03 README.md

Install step 1.2: Create a bashrc file for SKRIPS model::

  cp ./installOption_OTH/bashrc_comet ~/.bashrc_skrips
  
Then, add the following line to ~/.bashrc file::
  
  alias load_skrips="source ~/.bashrc_skrips" 
  
You'll have to activate the bashrc setups every time you want to run SKRIPS
model::
  
  load_skrips

The output is::
 
  Setting up the bashrc for SKRIPS model...
  SKRIPS_DIR is: /cw3e/mead/projects/csg102/rus043/scripps_kaust_model/
  
In the ~/.bashrc_skrips file, we have::

  module purge
  module load intel
  module load intelmpi
  module load netcdf
  
  # Location of the modules
  export SKRIPS_DIR=/cw3e/mead/projects/csg102/rus043/scripps_kaust_model/
  export ESMF_DIR=$SKRIPS_DIR/esmf/
  export WRF_DIR=$SKRIPS_DIR/WRFV413_AO/
  export MITGCM_DIR=$SKRIPS_DIR/MITgcm_c67m/
  export WW3_DIR=$SKRIPS_WAVE_DIR/ww3_607/
  
  # NETCDF and MPI
  export NETCDF=/opt/netcdf/4.6.1/intel/intelmpi/
  export NETCDF_DIR=/opt/netcdf/4.6.1/intel/intelmpi/
  export NETCDF_INC=/opt/netcdf/4.6.1/intel/intelmpi/include/
  export NETCDF_LIB=/opt/netcdf/4.6.1/intel/intelmpi/lib/
  export MPI_DIR=$MPIHOME
  export MPI_INC=$MPIHOME/include/
  export MPI_LIB=$MPIHOME/lib/

  # For WW3
  export WWATCH3_NETCDF=NC4
  export NETCDF_CONFIG=$NETCDF_DIR/bin/nc-config
  export PATH=$WW3_DIR/bin:${PATH}
  export PATH=$WW3_DIR/exe:${PATH}
  
  # For ESMF
  export ESMF_OS=Linux
  export ESMF_COMPILER=intel
  export ESMF_COMM=intelmpi
  export ESMF_OPENMP=OFF
  export ESMF_LAPACK=internal
  export ESMF_NETCDF=nc-config
  export ESMF_BOPT=g
  export ESMF_ABI=64
  export ESMF_YAMLCPP=OFF
  export ESMF_LIB=$ESMF_DIR/lib/lib$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk
  
  # Others
  export SKRIPS_NETCDF_INCLUDE=-I`nc-config --includedir`
  export SKRIPS_NETCDF_LIB=-L`nc-config --libdir`

  export LD_LIBRARY_PATH=$NETCDF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$MPI_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$ESMF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  
1. Please make sure the paths for the model components, NETCDF, MPI, and WW3 are correct, although WW3 is not necessary.
We will discuss the ESMF setups later. 
2. To install the coupled model on ring@ucsd, please use bashrc\_ring
3. To install the coupled model on Shaheen, please use bashrc\_shaheen


Now we can start install the module components and the coupled model:

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
   Install and test WW3<ww3/index>
   Install and test coupled code<coupled/index>
