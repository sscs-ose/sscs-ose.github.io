---
layout: page
permalink: /digital/
title: Digital IC Design
subtitle: All things related to open-source digital IC design
callouts: digital_ic
---

## Digital IC Design

As you read on our main page, digital design starts with a high-level textual description and ends with a layout representation in GDS format. The main steps along this route are detailed in the following chapters.

### Simulation

Hand-in-hand with creating the Verilog behavioral description is the simulation (for writing Verilog code, you just need a text editor).

The following Verilog simulators are available:

* [Icarus Verilog](https://github.com/steveicarus/iverilog.git)
* [Verilator](https://github.com/verilator/verilator)

We recommend Icarus Verilog (`iverilog`) for most simulation tasks, as it is easier to handle than `verilator`. However, if you want to simulate large designs, `verilator` is the way to go as it is much faster.

Both digital circuit simulators produce output results that can be viewed with [GTKWave](https://github.com/gtkwave/gtkwave), which is a waveform plot tool for digital simulation.

If you prefer to write VHDL, there is also a [simulator for that](https://github.com/ghdl/ghdl).

### Writing Verilog code

Writing good Verilog code is an art by itself and requires practice. There are a few online courses available. One possibility to get you started is this [tutorial](https://www.chipverify.com/verilog/verilog-tutorial), but there are many more—Google it!

You definitely should use [linting](https://en.wikipedia.org/wiki/Lint_(software)), which means using static code analysis to spot typical errors. Both `iverilog` and `verilator` support code linting, so use both.

For advanced digital designers using [code coverage analysis](https://github.com/hpretl/verilog-covered) might be an option.

### Synthesis

Once you have written behavioral code in Verilog, it needs to be translated to register-transfer level (RTL), which is essentially a gate netlist. This step is called synthesis, and the go-to tool for this step is [Yosys](https://github.com/YosysHQ/yosys).

The synthesis step is non-trivial, as there is a difference between the behavioral code that you can write in Verilog and a synthesizable code. We propose to run test syntheses in `yosys` and carefully watch the warnings and errors.

### RTL2GDS

Synthesis is also the first step in the [OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) RTL2GDS flow. This flow consists of a sequence of steps:

![OpenLane flow](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/docs/_static/openlane.flow.1.png?raw=true)

The multitude of tools is controlled by settings which are described [here](https://github.com/The-OpenROAD-Project/OpenLane/blob/master/configuration/README.md).

If everything goes well, then the flow runs without errors, especially when it comes to timing. Occasionally, your design might have setup- or hold-time violations that the tools can not fix automatically. In this case, you have to intervene manually, and [this document](https://docs.google.com/document/d/13J1AY1zhzxur8vaFs3rRW9ZWX113rSDs63LezOOoXZ8) can be a starting point where to look for a solution.

### Installation of Tools and PDK

The installation of OpenLane and the SKY130 PDK can be tricky but is described [here](https://github.com/The-OpenROAD-Project/OpenLane) (see "Setting Up OpenLane"). The proposed installation uses [Docker](https://www.docker.com), which is available for the major computing platforms.

Alternatively, several other solutions are available; you can check the Slack channel for options. For a curated, pre-installed Docker image containing PDK and a large number of tools, you can check [here](https://github.com/efabless/foss-asic-tools) or [here](https://github.com/hpretl/iic-osic-tools) (the later Docker image runs on `x86_64` and `aarch64`).

### Caravel SoC

Once your digital IP block is ready, you might want to create an entire IC and send it for fabrication. Getting from an IP block to a complete IC (including pads, ESD protection, power supply network, reset, and clocking circuitry) can be cumbersome.

Luckily, a framework exists, an "empty shell," so to speak, from [efabless.com](https://efabless.com), called [Caravel](https://github.com/efabless/caravel_user_project). On the efabless.com homepage, you can find various examples of earlier chip designs. We propose to take one of these designs as a starting base, e.g., [this one](https://github.com/hpretl/iic-audiodac-v1).
