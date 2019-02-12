#############
Main function
#############

The main function is the wrapper for the coupled code. It creates a top-level
ESMF Gridded Component to contain all other Components. The pseudo code of the
main function is::

    # initialize ESMF
    call ESMF_Initialize();

    # create empty gridded component
    call ESMF_GridCompCreate();

    # read configuration file
    call read_config();

    # register the components in the gridded component
    # (e.g., ocean, atmosphere, and others)
    call ESMF_GridCompSetServices();

    # initialize main function;
    call ESMF_GridCompInitialize();

    # run main function;
    call ESMF_GridCompRun();

    # finalize main function;
    call ESMF_GridCompFinalize();

    # finalize ESMF framework;
    call ESMF_Finalize();


