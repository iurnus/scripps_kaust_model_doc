############
Ocean module
############

The ocean module is called by its parent module and runs the MITgcm.  The
pseudo code of the ocean module is::

    module mod_esmf_ocn

      # set ocean services
      subroutine OCN_SetServices
        call NUOPC_CompDerive(NUOPC_SetServices)
        call NUOPC_CompSetEntryPoint(ESMF_METHOD_INITIALIZE, OCN_Init1)
        call NUOPC_CompSetEntryPoint(ESMF_METHOD_INITIALIZE, OCN_Init2)
        call NUOPC_CompSpecialize(NUOPC_SetClock, OCN_SetClock)
        call NUOPC_CompSpecialize(NUOPC_Label_Advance, OCN_run)
        call ESMF_GridCompSetEntryPoint(ESMF_METHOD_FINALIZE, OCN_final)
      end subroutine OCN_SetServices

      # set in/out fields
      subroutine OCN_Init1
        call NUOPC_Advertise(importState, "fields_in")
        call NUOPC_Advertise(exportState, "fields_out")
      end subroutine OCN_Init1

      # set grid data and initialize the fields
      subroutine OCN_Init2
        # initial the ocean in MITgcm
        call MIT_INIT();
        # set the ocean grid in ESMF
        call OCN_SetGridArrays();
        # initialize the ocean variables in ESMF
        call OCN_Initialize();
      end subroutine OCN_Init2

      # run ocean model
      subroutine OCN_run
        # send atmosphere surface fluxes to ocean model
        call OCN_get();
        # run ocean model
        call MIT_run();
        # send ocean variables to atmosphere model
        call OCN_put();
      end subroutine OCN_run

      # finalize ocean model
      subroutine OCN_final
        call MIT_final();
      end subroutine OCN_final

    end module

