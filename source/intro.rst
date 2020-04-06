Introduction
============

This document captures the methods, verification environment
architectures and tools used to verify the first two members CORE-V
family of RISC-V cores, the CV32E and CV64A.

The OpenHW Group will, together with its Member Companies, execute a
complete, industrial grade pre-silicon verification of the first
generation of CORE-V IP, the CV32E and CV64A cores, including their
execution environment [1]_. Experience has shown that “complete”
verification requires the application of both dynamic (simulation, FPGA
prototyping, emulation) and static (formal) verification techniques. All
of these techniques will be applied to both CV32E and CV64A.

License
-------

Copyright 2020 OpenHW Group.

The document is licensed under the Solderpad Hardware License, Version
2.0 (the "License"); you may not use this document except in compliance
with the License. You may obtain a copy of the License at:

https://solderpad.org/licenses/SHL-2.0/

Unless required by applicable law or agreed to in writing, products
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

See the License for the specific language governing permissions and
limitations under the License.

Definition of Terms
-------------------

+-------------+--------------------------------------------------------------------+
| Term        | Defintion                                                          |
+=============+====================================================================+
| CORE-V      | A family of RISC-V cores developed by the OpenHW Group.            |
|             | The CV32E and the CV64A are the first two members of that          |
|             | that family. The CV32E has two planned variants, the               |
|             | CV32E40P and CV32E40.                                              |
+-------------+--------------------------------------------------------------------+
| Member      | A company or organization that signs-on with the OpenHW            |
| Company     | Group and contributes resources (capital, people,                  |
| (MemberCo)  | infrastructure, software tools etc.) to the CORE-V                 |
|             | verification project.                                              |
+-------------+--------------------------------------------------------------------+
| Active      | An employee of a Member Company that has been assigned to          |
| Contributor | work on an OpenHW Group project.                                   |
+-------------+--------------------------------------------------------------------+
| Instruction | A behavioural model of a CPU. An ISS can execute the same          |
| Set         | code as a real CPU and will produce the same logical               |
| Simulator   | results as the real thing. Typically only “ISA visible”            |
| (ISS)       | state, such as GPRs and CSRs are modelled, and any                 |
|             | internal pipelines of the CPU are abstracted away.                 |
+-------------+--------------------------------------------------------------------+
| ELF         | Executable and Linkable Format, is a common standard file          |
|             | format for executable files. The RISC-V GCC toolchain              |
|             | compiles C and/or RISC-V Assembly source files into ELF            |
|             | files.                                                             |
+-------------+--------------------------------------------------------------------+
| SDK         | Software Developers Toolkit. Also, riscv-gcc-tool-chain.           |
|             | A set of software tools used to compile C and/or RISC-V            |
|             | assembler code into an executable format. In the case of           |
|             | the CV32E and CV64A, this includes the supported RISC-V            |
|             | ISA compliant instructions, plus a set of XPULP extended           |
|             | instructions.                                                      |
+-------------+--------------------------------------------------------------------+
| Verification| Code, scripts, configuration files and Makefiles used in           |
| Environment | pre-silicon verification. Typically a testbench is a               |
|             | component of the verification environment, but the terms           |
|             | are often used interchangeably.                                    |
+-------------+--------------------------------------------------------------------+
| Testbench   | In UVM verification environments, a testbench is a                 |
|             | SystemVerilog module that instantiates the device under            |
|             | test plus the SystemVerilog Interfaces that connect to the         |
|             | environment object. In common usage “testbench” can also           |
|             | have the same meaning as verification environment.                 |
+-------------+--------------------------------------------------------------------+
| $PROJ_ROOT  | Local path of a cloned copy of a GitHub repository. An             |
|             | example to illustrate:                                             |
|             |                                                                    |
|             | [prompt]$ cd /wrk/greg/openhw                                      |
|             |                                                                    |
|             | [prompt]$ git clone https://github.com/openhwgroup/core-v-verif    |
|             |                                                                    |
|             | Here $PROJ_ROOT is /wrk/greg/openhw/core-v-verif. Note             |
|             | that this is not a required shell variable – its use in this       |
|             | document is merely as a reference point for an absolute path to    |
|             | your working copy.                                                 |
+-------------+--------------------------------------------------------------------+

Conventions Used in this Document
---------------------------------

**Bold** type is used for emphasis.

Filenames and filepaths are in italics: *./cv32/README.md*.

CORE-V Genealogy
----------------

The first two members the OpenHW Group’s CORE-V family of RISC-V cores
are the CV32E and CV64A. Currently, two variants of the CV32E are
defined: the CV32E40P and CV32E40. The OpenHW Group’s work builds on
several RISC-V open-source projects, particularly the RI5CY and Ariane
projects from PULP-Platform. CV32E is a derived of the RI5CY
project [2]_, and CV64A is derived from Ariane [3]_. In addition, the
verification environment for CORE-V leverages previous work done by
lowRISC and others for the Ibex project, which is a fork of the
PULP-Platform’s zero-riscy core.

This is germane to this discussion because the architecture and
implement of the verification environments for both CV32E and CV64A are
strongly influenced by the development history of these cores. This is
discussed in more detailed in Section `3 <#anchor>`__.

Unless otherwise noted, the “previous generation” verification
environments discussed in this document come from one of the following
master branches in GitHub:

**RI5CY**:https://github.com/pulp-platform/riscv/tree/master/tb/core

**Ariane**:https://github.com/pulp-platform/ariane/tree/master/tb

**Ibex**:https://github.com/lowRISC/ibex/tree/master/dv

A Note About EDA Tools
----------------------

The CORE-V family of cores are open-source, under the terms of the
Solderpad Hardware License, Version 2.0. This does not imply that the
tools required to develop, verify and implement CORE-V cores are
themselves open-source. This applies to both the EDA tools such as
simulators, and specific verification components, such as Instruction
Set Simulators.

Often asked questions are “which tools does OpenHW support?”, or “can I
use an open-source simulator to compile/run a CORE-V testbench?”. The
short answer is that the CORE-V testbenches require the use of IEEE-1800
(2017) or newer SystemVerilog tools and that this almost certainly means
that non-commercial, open-source Verilog and SystemVerilog
compiler/simulators will not be able to compile/run a CORE-V testbench.

CORE-V verification projects are intended to meet the needs of
Industrial users and will therefore use the tools and methodologies
currently in wide-spread industrial use, such as the full SystemVerilog
language, UVM-1.2, SVA, plus code, functional and assertion coverage.
For these reasons users of CORE-V verification environments will need to
have access to commercial simulation and/or formal verification tools.

For historical reasons, the “core” testbench of the CV32E40P runs
using Verilator, an open-source software tool which translates a subset
of the SystemVerilog language to a C++ or SystemC cycle-accurate
behavioural model. Continued support for Verilator will be on a
best-effort basis, and the "core" testbench is not considered a production
verification environment that is capable of fully verifying the CV32 cores.

The specific EDA SystemVerilog simulators used by OpenHW are Metrics
**dsim** and Cadence **Xcelium**, so its a very safe bet that the
Makefiles will always support rules to compile/simulate with these
tools. Use of other commercial tools is predicated on community interest
and support.

