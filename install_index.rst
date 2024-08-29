############################
Install the model components
############################

The official repository of SKRIPS v2.01 is maintained on Github::

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v2.01

Now I am using the SDSC supercomputer EXPANSE as an example. To install the
SKRIPS model (for example, in /expanse/nfs/cw3e/csg/102/rus043/scripps_kaust_model/), 
we have to download the SKRIPS model and set up the bashrc file.

Install step 1.1: Download SKRIPS version from Github::

  # I am using my work folder as an example
  cd /expanse/nfs/cw3e/csg/102/rus043/
  wget https://github.com/iurnus/scripps_kaust_model/archive/v2.01.zip
  unzip v2.01.zip
  mv scripps_kaust_model-2.01 scripps_kaust_model
  cd scripps_kaust_model
  ls -l

You will see the following folders and files::

  drwxr-sr-x  8 rus043 csg102 57344 Oct  4 10:56 coupler
  drwxr-sr-x  4 rus043 csg102 57344 Oct  4 10:56 esmf_test
  drwxr-sr-x  2 rus043 csg102 57344 Oct  4 10:56 license_statements
  -rw-r--r--  1 rus043 csg102  2597 Oct 11 11:03 README.md

Install step 1.2: Create a bashrc file for SKRIPS model::

  cp ./installOption_OTH/bashrc_comet ~/.bashrc_skrips
  
Here I am copying the bashrc file that I used for COMET. There are also bashrc
files for other machines that can be used in *installOption_OTH* folder.

Then, add the following line to ~/.bashrc file::
  
  alias load_skrips="source ~/.bashrc_skrips" 
  
Now activate the bashrc setups in the command line. Also, you need to activate
the bachrc setups when you want to run SKRIPS model, otherwise the Linux system
will not remember what you did::
  
  load_skrips

After the bashrc file is successfully loaded. We get the following output::
 
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
  export SKRIPS_MPI_DIR=$MPIHOME
  export SKRIPS_MPI_INC=$MPIHOME/include/
  export SKRIPS_MPI_LIB=$MPIHOME/lib/

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
  export ESMF_MOD=$ESMF_DIR/mod/mod$ESMF_BOPT/$ESMF_OS.$ESMF_COMPILER.$ESMF_ABI.$ESMF_COMM.default/
  export ESMFMKFILE=$ESMF_LIB/esmf.mk
  
  # Others
  export SKRIPS_NETCDF_INCLUDE=-I`nc-config --includedir`
  export SKRIPS_NETCDF_LIB=-L`nc-config --libdir`

  export LD_LIBRARY_PATH=$NETCDF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$SKRIPS_MPI_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  export LD_LIBRARY_PATH=$ESMF_LIB/${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}
  
When you want to install the SKRIPS model on another machine::

1. Please make sure the paths for the model components, NETCDF, MPI, WW3, and ESMF are correct. The ESMF setups will be detailed in latter sections.
2. WW3 is not necessary for running the coupled model. 
3. To install the coupled model on ring@ucsd, please use installOption\_OTH/bashrc\_ring.
4. To install the coupled model on Shaheen, please use installOption\_OTH/bashrc\_shaheen.

Now we can start install the module components and the coupled model.

.. toctree::
   :maxdepth: 1
   :titlesonly:

   Install and test ESMF<esmf/index>
   Install and test MITgcm<mitgcm/index>
   Install and test WRF<wrf/index>
   Install and test WW3<ww3/index>
   Install and test coupled code<coupled/index>
