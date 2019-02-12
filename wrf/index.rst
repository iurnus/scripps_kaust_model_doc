####################
Install and test WRF
####################

The WRF ARW model is a fully compressible, nonhydrostatic model (with a hydrostatic option). The WRF
ARW model contains initialization programs (ideal.exe and real.exe), a numerical integration program
(wrf.exe), and a program to do one-way nesting (ndown.exe). The WRF 3 supports a variety of
capabilities, including:

* Real-data and idealized simulations
* Various lateral boundary condition options for both real-data and idealized simulations
* Full physics options
* Non-hydrostatic and hydrostatic (as runtime options)
* One-way, two-way nesting and a moving nest
* Applications ranging from meters to thousands of kilometers
  
.. toctree::
   :maxdepth: 1
   :titlesonly:


   Install WRF solver<install_WRF>
   Install WPS for pre-processing<install_WPS>
   Install NCL for post-processing<install_NCL>
   Pre-processing using WPS<pre_WPS>
   Test WRF using real cases<test_WRF_real>
   Post-processing using NCL<post_NCL>
