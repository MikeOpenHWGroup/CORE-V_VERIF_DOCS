Revision History
================

+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| Revision   | Date         | Author          | Org.           | Comment                                                     |
+============+==============+=================+================+=============================================================+
| V0.1       | 2020-01-08   | Mike Thompson   | OpenHW Group   | First published draft.                                      |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.2       | 2020-01-09   | Mike Thompson   | OpenHW Group   | Minor updates.                                              |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.3       | 2020-01-14   | Mike Thompson   | OpenHW Group   | Move all Verification Planning to Section 1.                |
|            |              |                 |                | Started Section 2.3, “CV32E Sim Verif Env”.                 |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.4       | 2020-02-07   | Mike Thompson   | OpenHW Group   | Moved Revision History to end of document.                  |
|            |              |                 |                |                                                             |
|            |              |                 |                | Add Section 1.4 “A Note About EDA Tools”.                   |
|            |              |                 |                |                                                             |
|            |              |                 |                | Significant restructuring of Section 3 & 4.                 |
|            |              |                 |                | Updated Illustration 1.                                     |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.5       | 2020-03-23   | Mike Thompson   | OpenHW Group   | Add new Section 6: Simulation Tests in the UVM Environments |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.6       | 2020-03-26   | Mike Thompson   | OpenHW Group   | Section 1.2: Added $PROJ_ROOT.                              |
|            |              |                 |                | Section 3.2.2: Fixed confusion between paths used in the    |
|            |              |                 |                | RI5CY and core-v-verif repositories.                        |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+
| V0.7       | 2020-mm-dd   | Mike Thompson   | OpenHW Group   | Working copy.                                               |
+------------+--------------+-----------------+----------------+-------------------------------------------------------------+

End Notes
=========
.. [1]
   Memory interfaces, Debug&Trace capability, Interrupts, etc.

.. [2]
   Note that CV32E is not a fork of RI5CY. Rather, the GitHub repository
   https://github.com/pulp-platform/riscv was moved to
   https://github.com/openhwgroup/core-v-cores.

.. [3]
   CV64A is not forks of the Ariane. The GitHub repository
   https://github.com/pulp-platform/ariane was moved to
   https://github.com/openhwgroup/core-v-cores.

.. [4]
   These assertions are embedded directly in the RTL source code. That
   is, they are not bound into the RTL from the TB using cross-module
   references. There does not appear to be an automated mechanism that
   causes a testcase or regression to fail if one or more of these
   assertions fire.

.. [5]
   Derived from the PULP platform SDK.

.. [6]
   The macro and assembly code shown is for illustrative purposes. The
   actual macros and testcases are slightly more complex and support
   debug aids not shown here.

.. [7]
   **$PROJ\_ROOT/cv32/tests/core/riscv\_tests/rv64ui/addi.S** in your
   local copy of the core-v-verif repository.

.. [8]
   Anyone with access to GitHub will be able to see the coverage results
   of CORE-V regressions.

.. [9]
   This does not change the recommendation made earlier in this document
   to continue developing new testcases on the existing RI5CY testbench
   in parallel.

.. [10]
   See the README at
   https://github.com/openhwgroup/core-v-verif/tree/master/cv32/tests/core
   to see what this does. Note that the User Manual for the Verification
   Environment, which explains how to write and run testcases, will be
   maintained there, not in the
   `core-v-docs <https://github.com/openhwgroup/core-v-docs/tree/master/verif>`__\ project
   which is home for this document.

.. [11]
   Those familiar with the RI5CY testbench may recall that random
   generation of C programs using
   `csmith <https://embed.cs.utah.edu/csmith/>`__ was supported. Csmith
   was developed to exercise C compilers, not processors, it is not
   supported in the CORE-V environments.

.. [12]
   See Section `6.1.1 <#anchor-12>`__, `below <#anchor-12>`__.

.. [13]
   Generation of illegal or malformed instructions is also supported,
   and will be discussed in a later version of this document.

.. [14]
   This is termed Execution Environment Interface or EEI by the RISC-V
   ISA.

.. [15]
   First week of January, 2020.

.. [16]
   OneSpin White paper: Assuring the Integrity of RISC-V Cores and SoCs.
   OneSpin Solutions, 2019.
