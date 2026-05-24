# TODO

## Pass 1 — CI Pipeline

- [x] Pipeline added
- [x] DRC: FAIL — 20 errors (shorting_items, courtyards_overlap, solder_mask_bridge, unconnected_items)
- [x] ERC: FAIL — 17 warnings (missing custom KMLib libraries, symbol mismatches)
- [ ] Fab: SKIPPED (blocked by ERC pre-flight)
- [ ] gen-kibot-index: SKIPPED
- [ ] deploy-pages: SKIPPED

### DRC Errors (20)

- shorting_items: nets /VFIELD, +12V, /slice_template/VIN, /+24V shorting (6 errors)
- courtyards_overlap (1 error)
- solder_mask_bridge: front/rear aperture bridges between different nets (6 errors)
- unconnected_items: missing connections (7 errors)

### ERC Warnings (17)

- Missing custom libraries: KMLib_Connectors, KMLib_Mounting, KMLib_IC_SMD, KMLib_IC_Power, KMLib_IC_Digital, KMLib_Boards_Modules, KMLib_IC_MCU_MPU, KMLib_Discrete_Semiconductors
- Symbol mismatches: CPC1002N (Relay_SolidState), LTV-817S (Isolator)

## Pass 2 — Pre-Fab Review

- [ ] Fix 6x shorting items (VFIELD, +12V, VIN, +24V nets)
- [ ] Fix 1x courtyard overlap
- [ ] Fix 6x solder mask bridge errors
- [ ] Fix 7x unconnected items
- [ ] Add missing KMLib custom libraries or remap symbols
- [ ] Verify BOM completeness and sourcing
- [ ] Confirm board outline and mounting holes
- [ ] Footprint verification against datasheets
- [ ] Design review sign-off
