#######################
Install and test MITgcm
#######################

The MITgcm code is the ocean solver in the coupled model. It is a numerical
model designed for studying the atmosphere, ocean, and climate. Its
non-hydrostatic formulation enables it to simulate fluid phenomena over a wide
range of scales; its adjoint capability enables it to be applied to parameter
and state estimation problems. The MITgcm code can also run using flexible
domain decomposition.

.. toctree::
   :maxdepth: 1
   :titlesonly:


   Install MITgcm<install_MITgcm>
   MITgcm test cases (gyros)<test_MITgcm>
   MITgcm test cases (California region)<test_MITgcm_coupler>
