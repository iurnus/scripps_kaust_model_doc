#####################
Install and test ESMF
#####################

The Earth System Modeling Framework (ESMF) is a high-performance, flexible
software infrastructure for building and coupling weather, climate, and related
Earth science applications. The ESMF defines an architecture for composing
complex, coupled modeling systems and includes data structures and utilities
for developing individual models.

The basic idea behind ESMF is that complicated applications should be broken up
into coherent pieces, or components, with standard calling interfaces. In ESMF,
a component may be a physical domain, or a function such as a coupler or I/O
system. ESMF also includes toolkits for building components and applications,
such as regridding software, calendar management, logging and error
handling, and parallel communications.

.. toctree::
   :maxdepth: 1
   :titlesonly:


   Install ESMF on ring<install_ring>
   Install ESMF on shaheen<install_shaheen>
   Test ESMF<test_ESMF>
   Known Errors <install_errors>
