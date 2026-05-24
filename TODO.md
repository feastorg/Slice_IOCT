# TODO

## KiBot CI/Docs Pipeline

- DRC job: pass (standalone)
- ERC job: fail (pre-DRC check finds 20 DRC errors)
- Fab job: fail (pre-DRC check finds 20 DRC errors)
- gen-kibot-index: skipped (depends on Fab)
- deploy-pages: skipped (depends on gen-kibot-index)

### DRC Errors (20)

- shorting_items: nets /VFIELD, +12V, /slice_template/VIN, /+24V shorting (6 errors)
- courtyards_overlap (1 error)
- solder_mask_bridge: front/rear aperture bridges between different nets (6 errors)
- unconnected_items: missing connections (7 errors)

### ERC Warnings (17, non-blocking)

- Missing custom libraries: KMLib_Connectors, KMLib_Mounting, KMLib_IC_SMD, KMLib_IC_Power, KMLib_IC_Digital, KMLib_Boards_Modules, KMLib_IC_MCU_MPU, KMLib_Discrete_Semiconductors
- Symbol mismatches: CPC1002N (Relay_SolidState), LTV-817S (Isolator)
