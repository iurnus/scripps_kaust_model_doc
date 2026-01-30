.. Don't be afraid to commit documentation master file, created by
   sphinx-quickstart on Tue Apr 30 15:18:34 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


##########################################################################
User Guide of Scripps-KAUST Regional Integrated Prediction System (SKRIPS)
##########################################################################

The Scripps-KAUST Regional Integrated Prediction System (SKRIPS) is an
open-source model for coupled atmosphere-ocean simulations.


The SKRIPS model is intended to compile and run on many different Unix/Linux
operating systems with little or no change. Minimally, you will need fortran
compiler, MPI, netCDF libraries, and up to 20 GB of disk space for installing
all these components:

- Atmosphere Solver: WRF (version 4.5.2)
- Ocean Solver: MITgcm (version c68r)
- Wave Solver: WaveWatch III (version 6.07.1)
- Driver (coupler): ESMF/NUOPC (version 8.6.0)

The SKRIPS v2.01 code is available at:

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v2.01

Getting Started
---------------
.. toctree::
  :maxdepth: 2
  :titlesonly:

  How to Install the Model Components <install_index>
  How to Run Coupled Simulations <tutorial/index>


For Developers
--------------
.. toctree::
  :maxdepth: 2
  :titlesonly:

  Introduction of the Implementations <implementations/index>
  Limitations and Known Issues <limitations/index>
