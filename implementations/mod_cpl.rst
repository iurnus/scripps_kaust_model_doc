##############
Coupler module
##############

The coupler module will interpolate ocean data and atmosphere data. The pseudo
code of the coupler module is::

    module mod_esmf_cpl

      # set coupler services
      subroutine CPL_SetServices
      
        call NUOPC_CompDerive(NUOPC_SetServices)
        call NUOPC_CompSpecialize(NUOPC_Label_ComputeRH, CPL_ComputeRH)
        call NUOPC_CompSpecialize(NUOPC_Label_ExecuteRH, CPL_ExecuteRH)
        call NUOPC_CompSpecialize(NUOPC_Label_ReleaseRH, CPL_ReleaseRH)

      end subroutine CPL_SetServices

      # initialize the coupler
      subroutine CPL_ComputeRH
        call NUOPC_ConnectorGet(srcFields, dstFields)
        call ESMF_FieldBundelGet(srcFields)
        call ESMF_FieldBundelGet(dstFields)
        call ESMF_FieldBundelRegridStore(srcFields, interDstFields)
        call ESMF_FieldBundelRegridStore(interDstFields, dstFields)
        call ESMF_StateAdd()
      end subroutine CPL_ComputeRH

      # run the coupler
      subroutine CPL_ExecuteRH
        call ESMF_FieldBundelRegrid(srcFields, interDstFields);
        call ESMF_FieldBundelRegrid(interDstFields, dstFields);
      end subroutine CPL_ExecuteRH

      # finalize the coupler
      subroutine CPL_ReleaseRH
        call ESMF_FieldBundelRegridRelease();
      end subroutine CPL_ReleaseRH

    end module

