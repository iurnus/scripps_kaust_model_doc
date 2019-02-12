#############
Parent module
#############

The parent module is called either by the main function, and it controls the
child modules. The pseudo code of the parent module is::

    module mod_esmf_esm

      # set ESM services
      subroutine ESM_SetServices
        call NUOPC_CompDerive(NUOPC_SetServices)
        call NUOPC_CompSpecialize(NUOPC_SetModelServices, ESM_SetModelServices)
        call NUOPC_CompSpecialize(NUOPC_SetRunSequence, ESM_SetRunSequence)
      end subroutine ESM_SetServices

      # set Model services
      subroutine ESM_SetModelServices
        # set the model components
        call NUOPC_DriverAddComp("ATM", ATM_SetServices)
        call NUOPC_DriverAddComp("OCN", OCN_SetServices)
        # set the coupler components
        call NUOPC_DriverAddComp("ATM-TO-OCN", CPL_SetServices)
        call NUOPC_DriverAddComp("OCN-TO-ATM", CPL_SetServices)
        call ESMF_GridCompSet(clock=esmClock)
      end subroutine ESM_SetModelServices

      # set Run sequences
      subroutine ESM_SetRunSequence
        # set the coupler components
        call NUOPC_DriverAddRunElement("ATM-TO-OCN")
        call NUOPC_DriverAddRunElement("OCN-TO-ATM")
        # set the model components
        call NUOPC_DriverAddRunElement("ATM")
        call NUOPC_DriverAddRunElement("OCN")
      end subroutine ESM_SetModelServices
    end module

