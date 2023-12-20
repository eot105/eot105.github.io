---
layout: page
title: "The LED Poker"
description: "Integrated hardware/software solution to improve an LED binning machine"
icon_img: "/Resource/LED_Poker/LED_Poker_Full_iso.png"
#permalink: /Nd-Glass/
---

# The LED Poker: Custom electromagnetic SMD LED handling solution

## TL;DR:
***
<br>
Owing to the strict safety standards required by an aviation product, an automated machine is developed by a contract engineering company to sort LEDs by their output intensities to a tighter tolerance than could be obtained from the manufacturer. LEDs would get stuck to the tip of the vacuum nozzle used to handle the LEDs as they were placed into gridded trays causing the machine to jam. Traditional positive pressure release was insufficient as the burst of air caused by the LED being released would disturb its neighbors. I designed and built the mechanical components, driving circuit, and firmware for an electromagnetic actuator that releases the LED without blowing the others out of the tray. This "LED Poker" allowed the machine to run at higher speeds, with higher accuracy and with little to no human oversight. These improvements increased the operational speed by at least 3 times and allowed the company to take on higher volume and higher value contracts with confidence of on-time delivery.

<figure>
<table>
    <tr>
        <td style="border: none; padding: 0px"><img src="/Resource/LED_Poker/LED_Poker_Full_iso.png" style="max-width:50%; min-width:250px; height:auto" class="left"></td>
        <td style="border: none; padding: 0px"><img src="/Resource/LED_Poker/LED_Poker_Side_by_Side.svg" style="max-width:20%; min-width:200px; height:auto" class="right"></td>
        <td style="border: none; padding: 0px"><img src="/Resource/LED_Poker/LED_Poker_Drop_Off.gif" style="max-width:50%; min-width:300px; height:auto" class="center"></td>
        
    </tr>
</table>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. Overview of the the LED Poker showing initial CAD, detailed cutaway operation of the electromagnet, and functional demonstration.</b></figcaption>
</figure>

## Background:
***
<br>
This project was completed in 2022 as part of an internship with a contract engineering/manufacturing company in Phelps NY. The company entered into a contract with an aviation technology company to assemble and test PCBs that made up part of an exterior aircraft lighting system. A high level view of the PCB produced by the company is shown in Fig 1:

<figure>
<center><img src="/Resource/LED_Poker/PCB_Overview.svg" style="max-width:50%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. Aircraft Lighting PCB showing normal LEDs, calibration LEDs, photodetectors, and microcontroller</b></figcaption>
</figure>

The PCB consists of a ring of high intensity LEDs. Over their lifetime LEDs will slowly degrade and their light output will decrease. Due to the safety critical nature and harsh operating conditions of aviation lighting, this degradation must be monitored and the light module must be replaced when the output level of the LEDs drops below some percent of their initial intensity.

The product designed by the aviation technology company consists of two photodetectors monitoring two LEDs. One measures the light output of an LED that is on as part of normal operation. The other measures the output of a separate LED that is only turned on briefly during system startup. When the system senses that the output of the normal operation LED has fallen below some set percentage of the calibration LED, it is assumed that all the LEDs are nearing the end of their life and the module is flagged for replacement. 

Due to the stringent nature of the regulations that this product is designed to meet, the two LEDs being monitored need to start with output intensities that are very closely matched so that a small deviation can be detected. It is not possible to purchase LEDs from the manufacturer that are binned to a tight enough tolerance. 

To solve this problem, the aviation technology company invested in the development and production of a custom machine that could automate the sorting of SMT LEDs into bins of tighter tolerance. The "LED binning machine" (Fig. 2) accepts a standard reel of LEDs form the manufacturer binned to some tolerance (e.g. &plusmn;10%). The LEDs are picked up with a vacuum nozzle and placed into a custom designed testing jig to have their light output measured. The machine then compares the current output value with the previous output values and determines which of several 3D-printed trays the LED should be placed into. The trays can then be loaded into the pick-and-place machine that assembles the PCB. The two LEDs that make up the lifetime checking system are chosen from this special tray and can be guaranteed to be within a tolerance of as high as &plusmn;1% when assembled. 

<figure>
<center><img src="/Resource/LED_Poker/Whole_Machine.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. The LED binning machine, reel feed mechanism is in the middle, integrating sphere is the black ball on the right, blue trays are loaded into the rest of the production line.</b></figcaption>
</figure>

## The Problem:
***
<br>
When I started working on this project the machine was working, but not very well due to a problem with the vacuum nozzle that was used to handle the LEDs. The LEDs consist of a lens made of soft, slightly sticky, resin. This lens would get stuck to the nozzle and would prevent the LED from being released. In a normal SMT workflow this problem is solved by feeding a small amount of positive pressure back through the nozzle to break the sticking force of the part to the tip. This works because the part is being placed onto a PCB with tacky solder paste which prevents it (and the other components surrounding it) from being blown away from this small puff of air pressure. For this application however, the LEDs are being placed in a tray with nothing to hold them down or prevent them from being being moved by the air. This led to a situation where if the release pressure was set too high, the LEDs already in the tray would be blown out of place, but set the pressure too low and the LED would not release properly and would jam the machine as it tried to pick up a new LED with one already in the nozzle. The solution when I was hired was to error on the side of too low, and have an intern sit by the machine with a pair of tweezers while it ran at 1/10 its maximum speed to manually help it release the LEDs into the proper space in the trays. 

<figure>
<center><img src="/Resource/LED_Poker/Sticky_LED.svg" style="max-width:50%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. The style of LED used on the PCB with the slightly sticky plastic resin lens visible.</b></figcaption>
</figure>

In addition to this being a waste of (even an intern's) time, the machine running at such a low speed and with such inconsistency meant that the company could not sort the LEDs fast enough to keep up with the rest of the production line. With contracts piling up and on time deliveries slipping further away, a better solution was desperately needed. 

## The Solution: 
***
<br>
To solve this problem I designed the "LED Poker", the CAD for this solution is shown in Fig 4:

<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Full_iso.png" style="max-width:75%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. LED Poker in CAD.</b></figcaption>
</figure>

The realized product mounted on the machine is shown in Fig 5:

<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Full.png" style="max-width:100%; min-width:200px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. LED Poker In real life and mounted on the machine. The large black integrating sphere on the right used to measure the LED intensity.</b></figcaption>
</figure>

Fig 6 shows the main components of the LED poker. The quick connect fitting on the top provides vacuum to suck the LED up into the nozzle. The ferrous slug, plastic fiber and return spring form the heart of the mechanism that moves to pop the LED out of the nozzle. The custom wound electromagnet slides over the machined nylon tube and is held in place by the spacer. The vacuum nozzle and rubber tip are off-the-shelf parts from a commercial SMT pick-and-place machine. The machined nylon tube has female threads on both ends to accept the quick connect fitting and the vacuum nozzle. The inside of the tube is machined with two simple step features to contain the return spring and allow the ferrous slug to slide freely.

<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Exploded_Labled.svg" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. LED Poker exploded view showing the major components.</b></figcaption>
</figure>

The ferrous slug has a small hole drilled into one end to accept the plastic fiber and three narrow slots cut down the outside of its length to allow vacuum to pass through to the end of the nozzle. When the LED Poker is picking up an LED from the reel, the spring holds the end of the plastic fiber away from the end of the nozzle. In this state the center of the slug is above the center of the electromagnet. After the LED has been measured and it is time to drop it off in the correct tray, the vacuum is turned off and vented and the electromagnet is energized pulling the center of the slug to match the center of the electromagnet. This in turn pushes the end of the plastic fiber onto to the top of the LED and gently pops it off the end of the nozzle, depositing it in the tray without disturbing any of its neighbors. 

<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Side_by_Side.svg" style="max-width:50%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. Cutaway of the LED Poker showing the electromagnet energized and de-energized.</b></figcaption>
</figure>

The end design of the LED Poker went through several iterations before the final working design was arrived at. The first designs used a 3D-Printed adapter to attach the electromagnet. The nylon tube ended up being significantly more vacuum tight and the slug slid much smoother inside it. Because the slug is so small, the amount of force exerted by the electromagnet is very weak, so a very weak spring is needed so as not to exert an insurmountable restoring force. The electromagnet also had to be made much larger to both exert more force on the slug and to increase the distance the slug can travel. 

The electromagnetic was wound in house by me using an electric drill, a bobbin made of a bolt and two laser cut spacers, and a spool of magnet wire. The plastic fiber was cut from one of the bristles of my roommate's broom. 

I created a simple driver circuit for the electromagnet consisting of a MOSFET and flyback diode, a MOSFET gate driver IC and a DC regulator to step down the +24VDC rail on the machine to 5V to power the gate driver. This circuit was soldered on a piece of perf-board and a digital signal line was run from the microcontroller that controls the whole machine. 

<figure>
<center><img src="/Resource/LED_Poker/Board_Mounted.png" style="max-width:75%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. Driver circuit for the LED Poker electromagnet mounted on the machine, with +24VDC for power, signal line from the microcontroller, and output lines to the electromagnet.</b></figcaption>
</figure>

I modified the firmware running on the main microcontroller to pulse the electromagnet control line high when during its LED drop-off routine. I also added some more advanced logic to use the inline vacuum sensor to ensure the LED was no longer stuck to the nozzle, and to attempt the release subroutine again if the first try failed. With the addition of a webcam and a remote enabled PC, I could keep tabs on the machine from my desk for the rare occasion something went wrong and needed human intervention. 

## Results:
***
<br>
<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Working_Loop_HD.gif" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. Demo of the LED Poker actuating. </b></figcaption>
</figure>

Did the LED Poker work? Hell yeah it did. The LED Poker allowed the machine to run at full speed with very minimal human interaction and extremely low error rates. Whereas before an order of a few hundred LEDs constituted a slight panic as this entailed a painful several day long endeavor, after the implementation of the LED Poker an order of 300 LEDs could be done before lunch. We could for the first time have a backlog of binned LEDs and could finally keep up with the rate the production line consumed them. In addition to freeing up everyone's time now that a full time LED binning babysitter was no longer necessary, this improvement also allowed the company to take on higher quantity, higher value, contracts with the confidence that they could be delivered on time.

<figure>
<center><img src="/Resource/LED_Poker/LED_Poker_Drop_Off.gif" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. The LED Poker releasing an LED into the tray.</b></figcaption>
</figure>

The image below shows ~2500 LEDs sorted and ready for use in the production line (each tray contains LEDs with light outputs within 1% of each other, an expected normal distribution emerges). These were finished over the course of 2-3 days and required only a few hours of labor. An order of this size would have taken weeks to finish before my improvements to the LED binning process. 

<figure>
<center><img src="/Resource/LED_Poker/Binned_LEDs.png" style="max-width:50%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment LED_Poker_fig_counter %}. ~2500 LEDs binned over the course of a few days.</b></figcaption>
</figure>
