---
layout: page
title: "Photonics IC Current Source"
description: "Precision current controlled DAC with 50 output channels for development and testing of silicon photonic circuits."
icon_img: "/Resource/MOSAICS/Mosaics_Ico.png"
#permalink: /Nd-Glass/
---

# M.O.S.A.I.C.S: Multi Output Synchronized Adjustable Independent Current Source
## High precision, open source current DAC designed for driving lasers in high density silicon photonic integrated circuits.        

# TL;DR:
***
<br>

MOSAICS is a high channel count current controlled DAC designed for driving laser sources and modulators used in cutting edge silicon photonic integrated circuits. It consists of a PCB built around the LTC2662 IC capable of driving 50 channels independently with up to 100uA resolution at 150 kHz update rate. MOSAICS communicates with a PC over full speed USB 2.0 for control and feeding back the actual current draw and voltage at each channel. Each channel can source up to 300mA and sink up to 80mA. The system also features a calibration port to account for any errors introduced by the signal conditioning of the current/voltage feedback. The system is currently in development, as of December 2023 the PCBs have been ordered and we are waiting on their arrival. 

<figure>
<center><img src="/Resource/MOSAICS/MOSAICS_Full_Crop.png" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. 3D render of the MOSAICS PCB.</b></figcaption>
</figure>

# The Problem:
***
<br>

Silicon photonics is, in general, any technology that integrates photonic processes (the study and science of light) with traditional CMOS processes (the technology that builds all modern transistors and computer chips). This emerging field promises many advancements in telecommunications, computation, and sensing. While many aspects are still in the research stage, silicon photonics is fast approaching commercial scalability especially in the telecommunications and computation sector, where ultra fast optical transceivers and CPU-Memory links are available for purchase now from companies like Intel and Ayar Labs. 

<figure>
<center><img src="/Resource/MOSAICS/PIC_Wide.svg" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. Wide shot of a photonic integrated circuit (red circle is detailed in Fig 2.).</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/MOSAICS/PIC_Detail.svg" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. 500x zoom in on region detailed with red circle in Fig 1.</b></figcaption>
</figure>

A representative photonic integrated circuit is shown in Fig 1, with a detailed view in Fig 2. The photonic waveguides are essentially microscopic fiber optic cables etched directly into the silicon. Integrating the optical elements and traditional silicon elements of a circuit in very close proximity is what gives silicon photonics its massive speed and bandwidth improvements over traditional integrated circuits. Photonic waveguides have the potential be manufactured at the same scale and density as CMOS transistors, allowing chips with networks containing billions of photonic waveguides to be manufactured. This opens up the possibility to create more advanced devices that take advantage of the unique properties of light. These include quantum devices that take advantage of entangled photon pairs, and photonic neural networks that perform matrix calculations using the interference of light. 

<figure>
<center><img src="/Resource/MOSAICS/Diode_IV.png" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. Standard diode I-V response curve.</b></figcaption>
</figure>

A common feature of these more advanced photonic devices is that they require a large number of independent light sources, which are almost without exception diode lasers. A laser diode is just like any other semiconductor diode, and a representative current vs voltage curve is shown in Fig 3, under forward bias the diode will not let current flow until the threshold voltage, Vd, is reached. At that point the current conducted by the diode will begin to increase exponentially and it will begin to produce light. If a laser diode allows too much forward current to flow (typically > 100mA for the size of diode used in silicon photonics research) it will damage and in the worst case destroy the device. In addition, the structure of a laser diode is such that the peak wavelength of the output is directly related to the forward current, so some tunability can be achieved simply by controlling the current through the diode. 

Because forward current has an exponential dependence on voltage, and because there will exist small manufacturing differences between different laser diodes, it is essentially impossible to use a fixed voltage source to control a laser diode. The amount of current the device is drawing would be unknown. Therefore the control scheme for a laser diode has to include current sensing feedback: the current draw from the voltage source is monitored and the voltage is adjusted to keep the current draw at some set-point. When developing advanced silicon photonic integrated circuits having ~500uA of current resolution is critical, as is the ability to interface the current control instrument to a program to automate testing and data acquisition. 

Lasers are not the only devices used in silicon photonic systems that require this kind of constant current control. To make photonic integrated circuits useful, the light often needs to be modulated. Various materials have optical properties such as their index of refraction than can be changed as a function of temperature. This is accomplished sometimes using a Thermoelectric Generator (TEG) when space permits, but is often done using a simple strip of metal deposited using standard CMOS processes onto the integrated circuit that acts as a micro resistive heater. A constant current source is required to drive either a TEG or a resistive heater, and MOSAICS can be used for this purpose as well, it is accurate enough to allow for fine tuning of heat sensitive processes, and capable of delivering enough power to be used with large TEGs or resistive heaters.

Power supplies that meet the above requirements are available commercially, and for controlling small numbers of devices these solutions work quite well. As photonic circuits get more advanced, however, the number of lasers or modulators that need independent control increases. A photonic neural network, for example, requires one laser for each of the input neurons and could require one modulator per neuron connection. A large network then can require 10's of devicesS, and more advanced ones still can require 100's. Commercial solutions that provide large numbers of independent current controlled output channels while meeting the aforementioned requirements are much harder to come by, some do exist, but their high cost and closed source software make them unappealing and impractical to groups preforming research at the university level. 

# The Solution:
***
<br>

Working with the integrated photonics group at the Rochester Institute of Technology, I am in the process of developing a solution that meets the needs of researchers: the Multi Output Synchronous Adjustable Independent Current Source or MOSAICS. MOSAICS is an integrated hardware/software solution with 50 independently controllable current sources each with a maximum resolution of 100uA. The current channels can be updated at a maximum rate of 150 kHz and can feedback both the current draw and the voltage at each output. The device will communicate over a standard ASCII protocol which can be implemented in any programming language, but a more user friendly API will be developed in Python. 

Several conversations were had with other researchers in the group who will in the future utilize this product to test their designs, as well as other stakeholders familiar with the field to understand what features the final product needs to have and what metrics it needs to meet. A survey of the various laser diodes and modulators in use today was taken to determine what the maximum current supply per channel should be, with some headroom from the future. The maximum resolution was determined based on the work of [Authors of the paper] in their paper that discusses the limits of DAC resolution in designing high loss MZIs in silicon photonics. 

MOSAICS is built around the LTC2662 current driver IC from Analog being driven with a daisy chained 50 MHz SPI bus from an STM32 microcontroller. Current/Voltage feedback is provided via a 12 channel analog multiplexer fed into an LTC2338 18 bit, fully differential high precision ADC. The ADC circuit features a calibration port to account for any error introduced by the voltage dividers and amplifiers that condition the input signal. The on board calibration allows a known reference voltage to be applied to each channel of the multiplexer and compared with the value reported by the ADC with the error stored in a look up table in memory. 

The MOSAICS PCB is designed as a 4 layer board with impedance controlled inner layers to increase the accuracy of the impedance matching simulations for the high speed USB and SPI signals. The PCB is designed allow banks of 10 channels to share a power supply so that the full current draw of the 50 channels can be split up. It also takes a 5V input to power the logic circuitry. The mounting holes are spaced to fit on a standard 1 inch grid, 1/4-20 tapped optical breadboard. The 50 pin ribbon cable that serves as the current outputs is used on several other projects in the RIT lab, and can easily be broken out as needed. The high current power source is designed to be supplied from a standard bench DC power supply, a ubiquitous photonics lab staple.

The BOM cost of the MOSAICS PCB comes out to approximately $10 per current channel, significancy less expensive than other options currently on the market. The hardware and software will also be released under an open source license to allow other research teams to adapt the product to their needs.

# Current State:
***
<br>

<figure>
<center><img src="/Resource/MOSAICS/MOSAICS_Layout.png" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. MOSAICS PCB layout.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/MOSAICS/MOSAICS_Layout_2.png" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. MOSAICS PCB layout detail on power input, MCU and current driver connections.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/MOSAICS/MOSAICS_Layout_3.png" style="max-width:50%; min-width:600px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment MOSAICS_fig_counter %}. MOSAICS PCB layout detail on output connector, ADC, and signal conditioning.</b></figcaption>
</figure>

As of December 2023 and after several months of prototyping, the MOSAICS PCBs have been ordered! When they are assembled I will update this page with notes on how the theoretical designs stack up in reality, and on the progression of the code development. 