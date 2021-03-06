Performance Counters
====================

Performance Counters in CV32E40P are placed inside the Control and Status
Registers and can be accessed with csrr and csrw instructions. See Table
9.1 for the address map of the performance counter registers

Machine/User Performance Counter Mode Register (PCMR)
-----------------------------------------------------

CSR Address: 0x7E1/0xCC1

Reset Value: 0x0000_0003

+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+------------+---------------+
| 31 | 30 | 29 | 28 | 27 | 26 | 25 | 24 | 23 | 22 | 21 | 20 | 19 | 18 | 17 | 16 | 15 | 14 | 13 | 12 | 11 | 10 | 09 | 08 | 07 | 06 | 05 | 04 | 03 | 02 | 01         | 00            |
+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+============+===============+
|    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    | Saturation | Global Enable |
+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+------------+---------------+

Detailed:

+---------+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| Bit #   | R/W   | Description                                                                                                                                           |
+=========+=======+=======================================================================================================================================================+
| 0       | R/W   | Global Enable: Activate/deactivate all performance counters. If this bit is 0, all performance counters are disabled. After reset, this bit is set.   |
+---------+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1       | R/W   | Saturation: If this bit is set, saturating arithmetic is used in the performance counter counters. After reset, this bit is set.                      |
+---------+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

Table 11: PCMR

Machine/User Performance Counter Event Register (PCER)
------------------------------------------------------

CSR Address: 0x7E0/0xCC0

Reset Value: 0x0000\_0000

+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+
| 31 | 30 | 29 | 28 | 27 | 26 | 25 | 24 | 23 | 22 | 21 | 20 | 19 | 18 | 17 | 16 | 15 | 14 | 13 | 12 | 11 | 10 | 09 | 08 | 07 | 06 | 05 | 04 | 03 | 02 | 01 | 00 |
+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+
|    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+

Detailed:

+-------------+-----------+-----------------+
| Bit #       | R/W       | Description     |
+=============+===========+=================+
| 16          | R/W       | TCDM_CONT       |
+-------------+-----------+-----------------+
| 15          | R/W       | ST_EXT_CYC      |
+-------------+-----------+-----------------+
| 14          | R/W       | LD_EXT_CYC      |
+-------------+-----------+-----------------+
| 20          | R/W       | FP_WB           |
+-------------+-----------+-----------------+
| 19          | R/W       | FP_DEP          |
+-------------+-----------+-----------------+
| 18          | R/W       | FP_CONT         |
+-------------+-----------+-----------------+
| 17          | R/W       | FP_TYPE         |
+-------------+-----------+-----------------+
| 16          | R/W       | CSR_HAZARD      |
+-------------+-----------+-----------------+
| 15          | R/W       | TCDM_CONT       |
+-------------+-----------+-----------------+
| 14          | R/W       | ST_EXT_CYC      |
+-------------+-----------+-----------------+
| 13          | R/W       | LD_EXT_CYC      |
+-------------+-----------+-----------------+
| 12          | R/W       | ST_EXT          |
+-------------+-----------+-----------------+
| 11          | R/W       | LD_EXT          |
+-------------+-----------+-----------------+
| 10          | R/W       | COMP_INSTR      |
+-------------+-----------+-----------------+
| 9           | R/W       | BRANCH_TAKEN    |
+-------------+-----------+-----------------+
| 8           | R/W       | BRANCH          |
+-------------+-----------+-----------------+
| 7           | R/W       | JUMP            |
+-------------+-----------+-----------------+
| 6           | R/W       | ST              |
+-------------+-----------+-----------------+
| 5           | R/W       | LD              |
+-------------+-----------+-----------------+
| 4           | R/W       | IMISS           |
+-------------+-----------+-----------------+
| 3           | R/W       | JMP_STALL       |
+-------------+-----------+-----------------+
| 2           | R/W       | LD_STALL        |
+-------------+-----------+-----------------+
| 1           | R/W       | INSTR           |
+-------------+-----------+-----------------+
| 0           | R/W       | CYCLES          |
+-------------+-----------+-----------------+

Table 8: PCER

Each bit in the PCER register controls one performance counter. If the
bit is 1, the counter is enabled and starts counting events. If it is 0,
the counter is disabled and its value won’t change.

In the ASIC there is only one counter register, thus all counter events
are masked by PCER and ORed together, i.e. if one of the enabled event
happens, the counter will be increased. If multiple non-masked events
happen at the same time, the counter will only be increased by one.

In order to be able to count separate events on the ASIC, the program
can be executed in a loop with different events configured.

In the FPGA or RTL simulation version, each event has its own counter
and can be accessed separately.

Performance Counter Counter Register (PCCR0-31)
-----------------------------------------------

CSR Address: 0x780 - 0x79F

Reset Value: 0x0000_0000

+----------------------------------+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+
| 31                               | 30 | 29 | 28 | 27 | 26 | 25 | 24 | 23 | 22 | 21 | 20 | 19 | 18 | 17 | 16 | 15 | 14 | 13 | 12 | 11 | 10 | 09 | 08 | 07 | 06 | 05 | 04 | 03 | 02 | 01 | 00 |
+==================================+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+====+
| Unsigned Integer Counter Value   |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |    |
+----------------------------------+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+

Table 9: PCCR0-31

PCCR registers support both saturating and wrap-around arithmetic. This
is controlled by the saturation bit in PCMR.

+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
|   Register     |   Name        |   Description                                                                                                                              |
+================+===============+============================================================================================================================================+
| PCCR0          | CYCLES        | Counts the number of cycles the core was active (not sleeping)                                                                             |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR1          | INSTR         | Counts the number of instructions executed                                                                                                 |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR2          | LD_STALL      | Number of load data hazards                                                                                                                |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR3          | JR_STALL      | Number of jump register data hazards                                                                                                       |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR4          | IMISS         | Cycles waiting for instruction fetches, i.e. number of instructions wasted due to non-ideal caching                                        |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR5          | LD            | Number of data memory loads executed.                                                                                                      |
|                |               |                                                                                                                                            |
|                |               | Misaligned accesses are counted twice                                                                                                      |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR6          | ST            | Number of data memory stores executed.                                                                                                     |
|                |               |                                                                                                                                            |
|                |               | Misaligned accesses are counted twice                                                                                                      |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR7          | JUMP          | Number of unconditional jumps (j, jal, jr, jalr)                                                                                           |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR8          | BRANCH        | Number of branches.                                                                                                                        |
|                |               |                                                                                                                                            |
|                |               | Counts taken and not taken branches                                                                                                        |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR9          | BTAKEN        | Number of taken branches.                                                                                                                  |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR10         | RVC           | Number of compressed instructions executed                                                                                                 |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR11         | LD_EXT        | Number of memory loads to EXT executed. Misaligned accesses are counted twice. Every non-TCDM access is considered external (PULP only)    |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR12         | ST_EXT        | Number of memory stores to EXT executed. Misaligned accesses are counted twice. Every non-TCDM access is considered external (PULP only)   |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR13         | LD_EXT_CYC    | Cycles used for memory loads to EXT. Every non-TCDM access is considered external (PULP only)                                              |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR14         | ST_EXT_CYC    | Cycles used for memory stores to EXT. Every non-TCDM access is considered external (PULP only)                                             |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR15         | TCDM_CONT     | Cycles wasted due to TCDM/log-interconnect contention (PULP only)                                                                          |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR16         | CSR_HAZARD    | Cycles wasted due to CSR access                                                                                                            |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR17         | FP_TYPE       | Cycles wasted due to different latencies of subsequent FP-operations                                                                       |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR18         | FP_CONT       | Cycles wasted due to contentions at the shared FPU (PULP only)                                                                             |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR19         | FP_DEP        | Cycles wasted due to data hazards in subsequent FP instructions                                                                            |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR20         | FP_WB         | Cycles wasted due to FP operations resulting in write-back contentions                                                                     |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+
| PCCR31         | ALL           | Special Register, a write to this register will set all counters to the supplied value                                                     |
+----------------+---------------+--------------------------------------------------------------------------------------------------------------------------------------------+

Table 104: PCCR Definitions

In the FPGA, RTL simulation and Virtual-Platform there are individual
counters for each event type, i.e. PCCR0-30 each represent a separate
register. To save area in the ASIC, there is only one counter and one
counter register. Accessing PCCR0-30 will access the same counter
register in the ASIC. Reading/writing from/to PCCR31 in the ASIC will
access the same register as PCCR0-30.

Figure 6 shows how events are first masked with the PCER register and
then ORed together to increase the one performance counter PCCR.

.. figure:: ../images/Events_PCCR_PCMR_PCER.png
   :name: events and pccr, pcmr and pcer
   :align: center
   :alt: 

   Figure 6: Events and PCCR, PCMR and PCER on the ASIC.
