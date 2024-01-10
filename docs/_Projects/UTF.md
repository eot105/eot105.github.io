---
layout: page
title: "Universal Test Fixture"
description: "Solution for quality control testing of arbitrary power supplies."
icon_img: "/Resource/UTF/UTF_Ico.png"
dates: "(2021-2022)"
page_order: 3
#permalink: /Nd-Glass/
---

# Universal Test Fixture (UTF): An automated, modular hardware/software solution for quality control assessment of DC power supplies

# TL;DR:
***
<br>
This project was completed over an 8 month internship with a contract engineering company that specializes in DC power supplies. Before shipment, individual power supplies need to be tested under a range of input voltages and load conditions. The company produces a variety of power supplies with different voltage and power ratings as well a variety of input/output connecter styles. As a result, a bespoke testing solution had to be developed for each model. I completed the development and implementation of the Universal Test Fixture (UTF). The UTF consists of a box fitted with a removable adapter PCB which can test a power supply with any connector style, sourcing a maximum of 300W per each of up to five outputs, using an arbitrary, software defined test sequence. As the lead on the project I debugged and revised schematics and PCB layout, developed low-level firmware and communication protocols, designed a custom XML based test sequence format, and designed a custom Graphical User Interface for control. The UTF allowed for test routines to be created for a new power supply in a matter of hours rather than weeks, in addition to allowing test procedures to be modified on the fly via software while allowing standardized data about each power supplies' performance to be stored in a central database for production tracking purposes. 

<figure>
<center><img src="/Resource/UTF/UTF_TLDR.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %}. The Universal Test Fixture fitted with an adapter PCB for testing a 30W AC/DC power supply.</b></figcaption>
</figure>

# Background:
***
<br>
A contract engineering company manufactures a large catalog of AC-DC power supplies, mostly for medical applications, for sale to an international clientele. The company also designs custom AC-DC and DC-DC power supplies based on client needs. As part of their quality assurance program the company tests every power supply that comes off the line before shipment. It is ensured that the output voltage of each supply remains within some agreed upon range under its full rated load and with the full range of AC input voltages the supply is rated for. The supplies are also tested to ensure their inrush current and voltage ripple are within acceptable ranges.

In addition to being used for pass/fail testing on the production line, long term data about power supply performance is used to track, diagnose, and improve production processes.

# The Problem:
***
<br>
Prior to the development of the UTF, power supplies were tested using bespoke test fixtures created as part of the design process for each supply. For some units with large order numbers, these were semi automated "bed of nails" style fixtures that would automatically apply load and measure output voltage. Fixtures for units with smaller production runs might consist of more rudimentary solutions such as a large resistor bolted to a piece of plywood, a multimeter, and an excel spreadsheet. There was little standardization between the construction or operation of these fixtures and testing often represented a significant source of delay in the process of shipping power supplies. In addition, it was difficult to adjust the test parameters on the fly and the data collected form the testing process lacked standardized formatting. 

# The Solution:
***
<br>
To address these shortcomings in the testing process the company invested in the development of the Universal Test Fixture, or UTF. The UTF is designed as a modular testing platform that can easily be adapted to test any power supply that could feasibly come off the line. It can test up to 5 outputs, each sourcing a maximum of 300W, can supply 90-250VAC or 0-300VDC input voltage, has 5 GPIO pins to test power supplies with digital enable circuitry, can measure DC output voltage to &plusmn; 10mV, 60Hz ripple voltage to 100 &mu;V and AC or DC inrush currents up to 400A. 

The only hardware design custom to each supply is an adapter PCB to interface the physical input and output connectors of the power supply with the generic bank of pin headers on the main control box. This PCB is designed from a standard template during the design process of the power supply. A software defined test procedure also needs to be created for the specific power supply being tested, this procedure defines what outputs should be put under what load and when, what input voltage the power supply should be tested at, and the tolerances for voltage outputs and inrush currents.


<figure>
<center><img src="/Resource/UTF/UTF_Overview.svg" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %} The assembled UTF system testing a 30W AC/DC power supply.</b></figcaption>
</figure>

An overview of the UTF is shown in Fig 0. The main control box contains the logic and power electronics to switch the inputs and outputs of the supply which is connected to the control box via the adapter PCB. The task of applying the necessary loads and measuring the output voltage is given to a bank of commercial bench instruments which communicate with a PC via ethernet: one electronic load per output and a single multimeter which is multiplexed to each output via an internal relay. The logic inside the main control box is controlled with an embedded microcontroller which communicates with the PC over UART serial. The PC is running a custom designed graphical user interface called the "Universal Test Fixture Control and Management Software", or UCAMS. UCAMS contains a sub-program called TestGen that assists with designing the software defined test procedures. Commands are sent to the Control box to select the appropriate input for the power supply, then to the electronic loads to apply the proper load condition. UCAMS then sends additional commands to the Control Box to switch internal relays to connect the multimeter to the power supply and measures the output voltage. UCAMS then determines if the voltage falls within the set tolerance and assigns the device a pass or fail.

When I took over the project in 2021 the main control box and PCB had been designed and built, albeit with several layout issues that needed addressing. I proceeded in developing the serial communication protocol and supporting firmware for the microcontroller that drives the main control box, developing the XML file standard for the software defined test procedures, establishing and troubleshooting the communication between UCAMS and the instruments, and developing the entire UCAMS and TestGen software suites. 

<figure>
<center><img src="/Resource/UTF/UTF_Inside.svg" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %} Inside the UTF main control box showing the AC transformer, microcontroller and switching relays.</b></figcaption>
</figure>

Fig 2, shows the internals of the main control box, the large transformer (bottom left) transforms 120VAC to 240VAC and 90VAC for testing international power supplies. The microcontroller is seen on the bottom center of the PCB with its blue UART cable. The bank of ten small reed relays (bottom right side) connect the output under test to the multimeter. Most of the rest of the components consist of power relays used to switch the main transformer.

<figure>
<center><img src="/Resource/UTF/UTF_Fixed_Board.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %}, the original UTF board with my fixed and updated board.</b></figcaption>
</figure>


The PCB in Fig 2 is the original one that was designed before I took over the project, it has several design issues that had to be bodged to get it to function. I fixed these issues and ran a new PCB layout, Fig 3 shows the original board (bottom) and my fixed board (top). 

The bulk of my contribution to the project involved the development of the UCAMS software suite. Written in python, it consists of the main UCAMS program and the "TestGen" program. The main UCAMS program has several responsibilities:
1. Load a user defined test sequence and create links between the hardware defined in the program and the physical hardware.
2. Communicate with the microcontroller inside the main control box over UART to:
    1. Switch the appropriate relays and energize the device under test as defined in the test sequence. 
    2. Connect the output of the device under test to the multimeter as defined in the test sequence.
    3. Log data from the inrush current sensor when the device is powered up.
3. Communicate with the electronic load(s) and multimeter over ethernet (using the SCPI protocol) to apply the correct load to and measure the correct output as defined by the test sequence. 
4. Compare the measurements from the device to the minimum and maximum values defined for that device in the test sequence. 
5. Give a visual indication that the device has passed or failed to the operator
6. Provide a detailed breakdown of each test condition the device was subjected to for on-the-fly debugging.
7. Save detailed testing information to a server for long term production line tracking.

The other part of the UCAMS suite, TestGen, performs the following tasks:
1. Allows the user to define the parameters of the device under test (maximum power, number of outputs, rated load, etc.)
2. Allows the user to set allowable tolerances for how high the output voltage can be under no load and how low it can drop under full load.
3. Allows the user to build a custom test sequence of device output conditions. For example: "Test the supply at 90VAC with both the 12V output and 5V output under no load, then under full load, then repeat with the supply powered at 120VAC"
4. Output the test sequence and power supply information to a portable file that can be loaded into the main UCAMS software. 

<figure>
<center><img src="/Resource/UTF/TestGen_Demo.gif" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %} Demonstration of creating a test sequence using the TestGen software.</b></figcaption>
</figure>

Fig 4 shows a quick demonstration of creating a test sequence for an example power supply with two inputs, designed to run from 120-240VAC.

The file TestGen creates is a standard XML file with custom defined headers. Although not a modern standard, XML was chosen for this application because it is platform agnostic, easy to create and parse programmatically using standard libraries, and is human readable which makes small manual tweaks to a procedure possible. 

<figure>
<center><img src="/Resource/UTF/UCAMS_Screenshot.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %} The UCAMS main screen after testing several single output power supplies.</b></figcaption>
</figure>

Fig 5 shows a screenshot of UCAMS during a real rest of a DC power supply. Each supply that is to be tested is inserted into the fixture, its serial number is typed or scanned in, and then the test sequence is executed either by clicking the run button in the lower right or by pressing the enter key. The result of the test is shown to the operator visually as a large green "check" or red "X" in the center of the program. The serial number and its pass/fail status is recorded in the test tree in the middle, each serial number can be expanded to see a detailed view of the exact conditions of each test cycle. The data is automatically saved as a CSV file to a central server after each test to avoid losing any data in the event of a crash or power failure. Individual tests can also be selected in the tree and exported as a CSV if desired. 

# Results:
***
<br>
I am proud to say that I led the UTF to a state of completion during my time with the company, when I left in 2022 two production lines were using my hardware and software to test all power supply units that came off the line, and there were several more control boxes being manufactured. I left the team with not only a working product and codebase but also with ample documentation and a roadmap of current issues and features in development. The UTF streamlined and standardized the testing and quality control of power supplies and allowed simple, robust and reliable test procedures to be developed along with the rest of the supply, rather than being an afterthought scrambled together at the end. I am particularly happy with the conversations and involvement I facilitated with both the production leadership and the employees who would actually be using the product to identify pain points with current practices and understand exactly what was needed in this solution and what was not. Working on the UTF was one of the first projects I got to be a part of that felt like it followed a true Quality design process from the prototype to the final line of code. Thanks to all who trusted me with its leadership!

<figure>
<center><img src="/Resource/UTF/UCAMS_Prototype.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment UTF_fig_counter %} Initial prototype sketches of layout of the UCAMS user interface.</b></figcaption>
</figure>

