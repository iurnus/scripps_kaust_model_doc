#########################
Post-processing using NCL
#########################

The post-processing code for the WRF example cases using NCL is available at:

* http://www2.mmm.ucar.edu/wrf/OnLineTutorial/Graphics/NCL/NCL_examples.htm

For the 2D hill case, use the wrf_Hill2d.ncl code. Copy the output obtained in the previous step::

    cp wrfout_d01_0001-01-01_00:00:00 wrfout_hill2d.nc

and, change the line in the code::

    line 17: type = “x11” -> type = “png"

Run the script to get the results::

    ncl wrf_Hill2d.ncl

More tutorials of the NCL is available at:

* https://www.ncl.ucar.edu/Document/Manuals/NCL_User_Guide/
* http://www.ncl.ucar.edu/overview.shtml
