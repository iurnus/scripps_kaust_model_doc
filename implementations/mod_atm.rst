#################
Atmosphere module
#################

The atmosphere module is called by its parent module and runs the WRF. The
pseudo code of the atmosphere module is::

    module mod_esmf_atm

      # set atmosphere services
      subroutine ATM_SetServices
        call NUOPC_CompDerive(NUOPC_SetServices)
        call NUOPC_CompSetEntryPoint(ESMF_METHOD_INITIALIZE, ATM_Init1)
        call NUOPC_CompSetEntryPoint(ESMF_METHOD_INITIALIZE, ATM_Init2)
        call NUOPC_CompSpecialize(NUOPC_SetClock, ATM_SetClock)
        call NUOPC_CompSpecialize(NUOPC_Label_Advance, ATM_run)
        # call ESMF_GridCompSetEntryPoint(ESMF_METHOD_FINALIZE, ATM_final)
      end subroutine ATM_SetServices

      # set in/out fields
      subroutine ATM_Init1
        call wrf_init()
        call wrf_getDecompInfo()
        call NUOPC_Advertise(importState, "fields_in")
        call NUOPC_Advertise(exportState, "fields_out")
      end subroutine ATM_Init1

      # set grid data and initialize the fields
      subroutine ATM_Init2
        # WRF--ESMF implementations will initialize the grid/field
        call wrf_state_populate();
      end subroutine ATM_Init2

      # run atmosphere model
      subroutine ATM_run
        # WRF--ESMF will get/put the data when running
        call wrf_run();
      end subroutine ATM_run

      # WRF can finalize itself?
      # subroutine ATM_final
      #   call wrf_final();
      # end subroutine ATM_final

    end module

