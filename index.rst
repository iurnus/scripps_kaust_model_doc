.. Don't be afraid to commit documentation master file, created by
   sphinx-quickstart on Tue Apr 30 15:18:34 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


####################################################################
Documentation of SKRIPPS-KAUST Regional Integrated Prediction System
####################################################################

This is a documentation for the SCRIPPS-KAUST Regional Integrated Prediction System (SKRIPS).  This
model is designed to be a state-of-the-art coupled atmosphere-ocean modeling system. It also
supports the new components by using the ESMF coupler and the NUOPC layer.

The designed modeling system currently includes the following models:

- Atmosphere Solver: WRF (version 3.9.1.1)
- Ocean Solver: MITgcm (version c66h)
- Driver (coupler): ESMF/NUOPC (version 7.0.0)

The features of the system includes:

- Multiple coupling time step
- Multiple execution styles

  * Concurrent Execution
  * Sequential Execution

Install the project requires the installation of MITgcm, WRF, ESMF, and their
dependencies. The instructions on the installation of the model components and
the coupler are detailed in the following sections. The examples on how to run
the coupled code and post-process the results are also presented.

Users can also extend this solver, utilities and libraries of this
coupled-solver, using some pre-requisite knowledge of the underlying method,
physics and programming techniques involved.

The coupled code is available at:

  https://github.com/iurnus/scripps_kaust_model.git

Now, let's start from installing and running the coupled code. Have fun! We will start from
installing the model components (ESMF, MITgcm and WRF). Maybe you are familar with these model
components, but we made a few modifications couple them. Let's go! 

.. toctree::
  :maxdepth: 2
  :titlesonly:

  Install model components <install_index>
  Coupled simulation tutorial <tutorial/index>
  Introduction of implementations <implementations/index>
  Limitations and known issues <limitations/index>
