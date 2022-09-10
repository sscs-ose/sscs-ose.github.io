---
permalink: /tapeout/
title: IC Tapeout Procedure
subtitle: All things related to getting an IC design ready for wafer production
callouts: analog_ic
---

Preparing an IC layout for production in a wafer foundry is a lengthy process. It usually involves quite a few checks, and once all are passed, and the IC layout is complete with pads, seal ring, etc., then the resulting layout database is sent to the foundry, usually in the form of a [GDS2](https://en.wikipedia.org/wiki/GDSII) or [OASIS](https://en.wikipedia.org/wiki/Open_Artwork_System_Interchange_Standard) file.

In the [Chipathon](https://sscs.ieee.org/about/solid-state-circuits-directions/sscs-pico-design-contest) we are using the service of [efabless.com](https://efabless.com). They have prepared an infrastructure chip including pads, ESD, programming infrastructure, and so on, called [Caravel](https://github.com/efabless/caravel_user_project) (for digital IC) or [Caravel Analog](https://github.com/efabless/caravel_user_project_analog) (for analog IC). In this [document](https://docs.google.com/document/d/1Lhw2pqrtm9ZJmXbdrflb0DODGaacu7CoabqjQpmeUZQ/edit), you can find some preliminary documentation on how to prepare your design using these infrastructure chips for TO at efabless.com.

Since IC fabrication is costly (tens of thousands of dollars reaching into millions depending on process node) and time-consuming (the turn-around time from submitting design data to receiving a prototype is a few months) it is of utmost importance to make sure that the implemented design is error-free. One way to ensure this (besides extensive simulation on all levels of the design hierarchy) is to have a formal design review before the tape-out, where several engineers review the design, layout, simulation results, etc. In this [TO checklist](https://docs.google.com/document/d/14fVJJB9oHs8LzoYUo322H58PTyj044MYgDIjb1f664o/edit), a few important considerations are collected.
