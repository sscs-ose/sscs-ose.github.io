---
permalink: /analog/
title: Analog IC Design
subtitle: All things related to open-source analog, mixed-signal, and radio-frequency IC design
callouts: analog_ic
---

## Analog IC Design

As outlined on our main page, analog design is mostly a manual process. Starting with drawing a schematic by hand in a schematic editor, the resulting netlist is simulated in a SPICE-level simulator, and the resulting output is checked for performance. If the design meets specifications, the IC layout is manually drawn in a graphical layout editor and checked for correctness.

These steps are detailed in the following sections.

## Schematic Entry

The recommended schematic entry tool is [Xschem](https://github.com/StefanSchippers/xschem). It is well integrated with the SKY130 PDK, and there is excellent support from the lead developer on Slack.

`xschem` comes with many tutorials and examples, which you can find [here](http://repo.hu/projects/xschem/xschem_man/xschem_man.html).

## Simulation

Simulation of a circuit is based on a netlist generated from the graphical representation in `xschem` and uses the component models part of the PDK.

There are mainly two simulators available that are historically based on [SPICE](https://en.wikipedia.org/wiki/SPICE):

* [nspice](http://ngspice.sourceforge.net) integrates well with `xschem` and `SKY130`.
* [Xyce](https://github.com/Xyce/Xyce) is a high-speed simulator with good multithreading support, but there are some incompatibilities in the `SKY130` ecosystem.

These simulators produce either a text file as output or a binary output format that can be viewed either directly in `xschem` or using [Gaw](https://github.com/StefanSchippers/xschem-gaw).

For Python-savvy designers, [spyci](https://github.com/gmagno/spyci) might be an attractive option for viewing and manipulating SPICE result files.

In order to get started quickly in `ngspice` analog simulations, this online version of simulation and plotting might be a good start: [EEsim.dev](https://eesim.dev). The source code is available [here](https://github.com/danchitnis/EEsim) for self-hosting.

## Layout Editing

Once the circuit is working to expectation, an IC layout has to be drawn that implements the circuit in question using MOSFETs, resistors, capacitors, and (potentially) inductors. For creating a full-custom chip layout, two tools are integrated into the `SKY130` environment:

* [Magic VLSI](https://github.com/RTimothyEdwards/magic) is an old tool but still the best option for layout creation. The operation of `magic` is a bit special, and you must go through the [tutorials](http://opencircuitdesign.com/magic) to understand the usage model. As with any advanced tool, you must effectively use mouse and keyboard shortcuts. This [cheat sheet](https://github.com/hpretl/iic-osic/blob/main/magic-cheatsheet/magic_cheatsheet.pdf) can help you to get started. This [step-by-step example of constructing a CMOS inverter](https://docs.google.com/document/d/1hSLKsz9xcEJgAMmYYer5cDwvPqas9_JGRUAgEORx1Yw/edit) might be a good start as well.
* [KLayout](https://www.klayout.de) is a more modern tool and fast working with large GDS files. However, since it lacks parametric layout cells (p-cells) for `SKY130` at the moment, its role is mainly for GDS viewing and layer manipulation.

Custom IC layout needs to be learned, so look for guidance and tutorials, e.g., on YouTube, or find a community for support.

Once you have finished your IC layout, you must check it for correctness. The following checks are the bare minimum:

* Design Rule Check (DRC) ensures that your layout conforms to the rules set up by the wafer foundry and ensures manufacturability. DRC can be done using `magic`, but the setup can be tricky. Refer to a ready-made script like [this one](https://github.com/hpretl/iic-osic/blob/main/iic-drc.sh) to get started.
* Layout-Versus-Schematic (LVS) ensures that the device netlist (aka "the circuit") that you drew in the layout tool is the same that you entered into the schematic tool (in terms of topology and component values). To check your layout, a netlist extracted from the layout (using `magic`) and a netlist extracted from the schematic (using `xschem`) are compared. This comparison tool is called [netgen](https://github.com/RTimothyEdwards/netgen). Again, setting this up can be tricky, but a [script](https://github.com/hpretl/iic-osic/blob/main/iic-lvs.sh) is also available for that.

Finally, you have to realize that the wires connecting microelectronic components are really thin (sub-µm) and packed close together. This increases wiring resistance, as well as capacitive coupling. Both are unwanted effects and can have a catastrophic impact on your circuit performance.

It is thus good practice to extract a netlist from the layout consisting of the wanted components (your circuit) and the unwanted ones (the parasitic elements introduced by the wiring). Luckily, `magic` can do this for you, and the resulting netlist (which consists of *many* components) is then re-simulated with the tool of your choice. Luckily, there is a [script](https://github.com/hpretl/iic-osic/blob/main/iic-pex.sh) for that as well.

### Installation of Tools and PDK

As you can imagine, setting up all these tools is involved, and there are various How-Tos available on the internet. The quick solution is to use a ready-made Docker container, as provided by the Institute for Integrated Circuits at Johannes Kepler University ([GitHub link](https://github.com/hpretl/iic-osic-tools)). An alternative for self-installation is the scripts provided by @proppy ([GitHub link](https://github.com/proppy/conda-eda/releases/)).

Of course, there are many more possibilities to set up your personal EDA environment.
