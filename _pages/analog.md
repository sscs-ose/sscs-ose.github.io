---
layout: page
permalink: /analog/
title: Analog IC Design
subtitle: All things related to open-source analog, mixed-signal and radio-frequency IC design
callouts: analog_ic
---

## Analog IC Design

As outlined in our main page, analog design is mostly a manual process. Starting with drawing a schematic by hand in a schematic editor, the resulting netlist is simulated in a SPICE-level simulator, and the resulting output checked for performance. If the design is meeting specifications, the IC layout is manually drawn in a graphical layout editor, and checked for correctness.

This steps are detailed in the following sections.

## Schematic Entry

The recommended schematic entry tool is [Xschem](https://github.com/StefanSchippers/xschem). It is well integrated with the SKY130 PDK, and there is excellent support by the lead developer on Slack.

`xschem` comes with many tutorials and examplex, which you can find [here](http://repo.hu/projects/xschem/xschem_man/xschem_man.html).

## Simulation

Simulation of a circuit is based on a netlist which is generated from the graphical representation in `xschem` and uses the component models wich are part of the PDK.

There are mainly two simulators available which are historically based on [SPICE](https://en.wikipedia.org/wiki/SPICE):

* [nspice](http://ngspice.sourceforge.net) is very-well integrated with `xschem` and `SKY130`.
* [Xyce](https://github.com/Xyce/Xyce) is a very fast simulator with good multithreading support, but there exist some incompatibilities in the `SKY130` ecosystem at this point.

These simulators produce either textfiles as output, or a binary output format which can be viewed either directly in `xschem`, or using [Gaw](https://github.com/StefanSchippers/xschem-gaw).

For Python-savvy designers [spyci](https://github.com/gmagno/spyci) might be an interesting options for vieweing and manipulating SPICE result files.

## Layout Editing

Once the circuit is working to expectation, an IC layout has to drawn which implements the circuit in question using MOSFETs, resistors, capacitors, and potentially inductors. For creating a full-custom chip layout, two tools are integrated into the `SKY130` environment:

* [Magic VLSI](https://github.com/RTimothyEdwards/magic) is an old tool, but still the best option for layout creation. The operation of `magic` is a bit special, and you definitely have to go through the [tutorials](http://opencircuitdesign.com/magic) to understand the usage model. As with any advanced tool, you have to use mouse and keyboard shortcuts effectively. This [cheatsheet](https://github.com/hpretl/iic-osic/blob/main/magic-cheatsheet/magic_cheatsheet.pdf) can help you to get started.
* [KLayout](https://www.klayout.de) is a more modern tool, and very fast to work with large GDS files. However, since it lacks parametric layouy cells (pcells) for `SKY130` in the moment, its role is mainly for GDS viewing and layer manipulation.

Custom IC layout is something that needs to be learned, so look for guidance and tutorials e.g. on YouTube, or try to find a community for support.

Once you are finished with your IC layout you need to check it for correctness. The following checks are the bare minimum:

* Design Rule check (DRC) makes sure that your layout conforms to the rules setup by the wafer foundry, and ensures manufacturability. DRC can be done using `magic`, but the setup can be tricky. Refer to ready-made script like [this one](https://github.com/hpretl/iic-osic/blob/main/iic-drc.sh) to get you started.
* Layout-versus-schematic (LVS) ensures that the device netlist (aka "the circuit") that you drew in the layout tool is the same that you entered into the schematic tool (in terms of topology and component values). In order to check this, a netlist extracted from the layout (using `magic`) and a netlist extracted from the schematic (using `xschem`) are compared. This comparison tool is called [netgen](https://github.com/RTimothyEdwards/netgen). Again, setting this up can be tricky, but there is also a [script available for that](https://github.com/hpretl/iic-osic/blob/main/iic-lvs.sh).

Finally, you have to realize that the wires connecting microelectronic components are really thin (sub-µm) and really packed close together. This increases wiring resistance, as well as capacitive coupling. Both are unwanted effects, and can have catastrophic impact on your circuit performance.

It is thus good practice to extract a netlist from the layout consisting of the wanted components (your circuit) and the unwanted ones (the parasitic elements introduced by the wiring). Luckily, `magic` can do this for you, and the resulting netlist (which consists of *many* components) is then re-simulated with the tool of your choice. Luckily, there is also [script](https://github.com/hpretl/iic-osic/blob/main/iic-pex.sh) for that as well.

### Installation of Tools and PDK

As you can imagine, setting up all this tools is involved, and there are various How-To available on the internet. The quick solution is to use a ready-made Docker container, which you can find either [here](https://github.com/efabless/foss-asic-tools) or [here](https://github.com/hpretl/iic-osic-tools) (the later Docker image runs on `x86_64` as well as `aarch64`).

Of course, there are many more possibilities out there to setup your personal EDA environment.
