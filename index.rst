.. Don't be afraid to commit documentation master file, created by
   sphinx-quickstart on Tue Apr 30 15:18:34 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


####################################################################
Documentation of Scripps-KAUST Regional Integrated Prediction System
####################################################################

This is a documentation for the Scripps-KAUST Regional Integrated Prediction System (SKRIPS).  This
model is designed to be a state-of-the-art coupled atmosphere-ocean modeling system. It also
supports the new components by using the ESMF coupler and the NUOPC layer.

The designed modeling system currently includes the following models:

- Atmosphere Solver: WRF (version 4.1.3)
- Ocean Solver: MITgcm (version c67m)
- Driver (coupler): ESMF/NUOPC (version 8.0.0)

Install the coupled model requires MITgcm, WRF, ESMF, and their dependencies. The installation
instructions are detailed in the following sections. A few examples on the coupled code are also
presented.

Users can also extend this solver, utilities and libraries of this coupled-solver, using some
pre-requisite knowledge of the underlying method, physics and programming techniques involved.

The SKRIPS v1.1 code is available at:

  https://github.com/iurnus/scripps_kaust_model/releases/tag/v1.1

Now, let's start from installing the coupled code. Let's go! 

.. toctree::
  :maxdepth: 2
  :titlesonly:

  Install model components <install_index>
  Coupled simulation tutorial <tutorial/index>
  Introduction of implementations <implementations/index>
  Limitations and known issues <limitations/index>
