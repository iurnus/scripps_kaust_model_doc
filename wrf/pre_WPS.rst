.. _pre_wps:

########################
Pre-processing using WPS
########################

Use WPS for pre-processing
==========================

Before running geogrid, in *namelist.wps* file, make sure that *geog_data_path* variable set to the
location where you put your geography static data (for ring, the location is
*/home/rus043/wrf/data/*). Once that is set, you can run geogrid::

    ./geogrid.exe >& log.geogrid

If you successfully created a geo_em* file for each domain, then you are ready to prepare to run
ungrib. Start by linking in the input GFS data (for ring, the location is
*/home/rus043/wrf/data/ds609.2*)::

    ./link_grib.csh path_where_you_placed_GFS_files

Then link to the correct Vtable (GFS, for this case)::

    ln -sf ungrib/Variable_Tables/Vtable.GFS Vtable

    # For grib1 ds609.2 data (before 01/01/2013)
    ln -sf ungrib/Variable_Tables/Vtable.AWIP Vtable
    # For grib2 ds609.2 data (after 01/01/2013)
    ln -sf ungrib/Variable_Tables/Vtable.NAM Vtable

Then run the ungrib executable::

    ./ungrib.exe

You should now have files with the prefix "FILE" (or if you named them something else, they should
have that prefix)

You are now ready to run metgrid::

    ./metgrid.exe >& log.metgrid

You should now have files with the prefix met_em* for each of the time periods for which you are
running.

How to download the wrf geography static data?
==============================================

The WRF geography static data is available here:

http://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html


How to download the ds609.2 data?
=================================

The ds609.2 data is available here:

https://rda.ucar.edu/datasets/ds609.2/#!access


How to download other data?
===========================

The following free datesets and the corresponding VTables are available:

http://www2.mmm.ucar.edu/wrf/users/download/free_data.html


