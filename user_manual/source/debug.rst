Debug
=====

| RI5CY supports the RISC-V debug specification 0.13 and it implementes
  the execution based to reuse the existing core pipeline.
| RI5CY has a **debug\_req\_i** input port that is sent by the system
  Debug Module. Such request makes the core jumps to the a specific
  address location where the Debug Rom is mapped. Such address location
  is referred as to the parameter DM\_HaltAddress. RI5CY implements the
  debug sets of registers as dpc, dcsr, dscratch0, dscratch1.
