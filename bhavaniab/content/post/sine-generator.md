+++
author = "Bhavani Anantapur Bache"
title = "Sine generator on FPGA-The Direct Digital Synthesizer"
date = "2015-06-30"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
featured = true
tags = [
    "DSP",
    "FPGA",
]
categories = [
    "DSP",
    "FPGA",
]
series = ["FPGA"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/sine1.jpeg"
+++

“I need a sine generator for my module.  How do I generate sine/cosine signals, when the module has to be implemented in hardware?”.

The interesting question was asked by a student who was pursuing masters in VLSI design. She was trying to build a demodulator module for Binary Phase shift Keyed (BFSK) signals on the FPGA. 
Here is the blog post that gives a detail description of sine/cosine generators, that can be implemented on the FPGA.

Sine generators are one of the important components in many of the DSP applications. They are used in modulators, demodulators, up converters, down converters, etc.

The output of the sine/cosine wave is defined as:
O(t) = sin((2*pi*frequency*t) + Phase)
where the ‘frequency’ represents the desired output frequency of a sine wave in Hertz and Phase represents a phase offset in radians per second.
A common method for digitally generating a sine/cosine wave is to employ a look up table scheme. The look up table stores samples of a sine wave.  A digital integrator is used to generate a suitable phase argument that is mapped by the look up table to generate the output waveform.
<h3>Operation of the DDS block</h3>
The figure gives an overview of the Xilinx DDS compiler core. D1 and A1 constitute the phase accumulator, which contains collection of components used to generate precise address which is given as the input to the look up table. The phase accumulator consists of an input phase angle or the “tuning word” which specifies the phase increment. The Phase register D1, stores the previous phase angle. The adder A1 adds the previous phase angle stored in D1 with the input tuning word to generate the actual phase. Quarter wave symmetry of sine wave can be exploited to represent the waveform using shorter look up tables. The quantizer block Q1 is used to map the actual value of phase to the shortened tables. This value is presented at the input address port of look up table T1.
<h3>Computation of the output frequency of DDS Block</h3>
The output frequency fout of DDS block is the function of FPGA system clock frequency fclk, the number of bits in the phase accumulation Bθ(n) and the phase increment value Δθ.
