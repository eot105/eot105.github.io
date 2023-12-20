---
layout: page
title: "Flashlamp pumped Nd:Glass laser"
description: "Design and build of a homemade 300J solid state flashlamp pumped Nd:Glass laser."
icon_img: "/Resource/FLP_NdGlass/Laser_Shot_Sparks_Ico.png"
#permalink: /Nd-Glass/
---

# FLP-Nd-Glass: Homebrew flashlamp-pumped solid-state Nd:Glass LASER.
***
<br>
<figure>
<center><img src="/Resource/FLP_NdGlass/washer_shot_sparks.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. The beam focused on a stainless steel washer.</b></figcaption>
</figure>


This project was completed in Aug of 2021. The result of 3 years of research, experimentation, failure, and learning. 

The first coherent photons were observed around 10 pm on Aug 8th, 2021 in a friend's lake house outside of Syracuse NY where I was staying over the summer (Thanks Christy, for not kicking me out when you found a laser in your living room). The first shot was a barely visible mark on a piece of laser alignment paper. Over the following days, with more tuning and alignment I was able to extract what was likely the maximum output of the laser given the flashtube and caps I have access to. When focused, the beam has no issue passing the standard test for homebrew lasers, burning through a carbon steel razor blade. 

## System Overview
***
<br>
<figure>
<center><img src="/Resource/FLP_NdGlass/full_system.JPG" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Full laser system showing the pump head, rod, flashtube, trigger circuit, and various optics.</b></figcaption>
</figure>

This laser uses a soviet era Nd:Glass rod as a laser medium, pumped with a Xenon flashlamp running at about 350J. The rod and lamp are housed in a custom machined elliptical aluminum pump head mounted on a custom optical bench that holds the mirrors and lens. The rod is powered from a bank of eight polymer film caps with a total capacitance of 580&mu;F charged with a neon sign transformer to 1150V. The lamp is externally triggered using a thin nickel wire and a trigger circuit consisting of a smaller 300V electrolytic cap, an SRC, and a car ignition coil. 

## Laser rod
***
<br>
The laser rod is surplus soviet era neodymium-doped silicate glass marked "GLS-1" with an [Nd<sup>3+</sup>] concentration of 1.9 * 10<sup>20</sup> cm<sup>-3</sup> (about 2%). This rod was an e-bay find and was re-machined by the seller, supposedly by a professional optics shop. This refinishing left the rod with an odd size of 11mm by 92mm. 

<figure>
<center><img src="/Resource/FLP_NdGlass/rod_altoids.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. End on view of rod showing exceptional optical clarity.</b></figcaption>
</figure>

## Pump head
***
<br>
Partly owing to this nonstandard size, the rod was housed in a custom-machined elliptical chamber. The design consisted of two parts, the clamshell-style pump head that held the rod and flashlamp at the two foci of an ellipse, and a baseplate that held the pump head and mirrors in place. The parts were custom machined out of 6061 aluminum and the inside of the pump head was polished to a mirror finish to give the best optical coupling.

<figure>
<center><img src="/Resource/FLP_NdGlass/cad.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Pump head CAD drawing showing the location of the rod and lamp on the two foci.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/FLP_NdGlass/mirror.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Polished pump head.</b></figcaption>
</figure>

## Flashlamp
***
<br>
The flashlamp used for the laser went through several iterations. I started with a soviet era tube about the same vintage as the rod (the IFP-800), there is some info on this tube at [Don Klipstein's flash lamp page](http://donklipstein.com/xea.html). I had an awful time getting this tube to work, the seals on the glass are dated and prone to cracking. In addition, this tube is almost comically long compared to the active region, which meant the baseplate had to be much longer than was needed. This also brought the ends of the tube too close to the mirror mounts resulting in some nasty arcing problems. Some of these tubes (mine included) have a dichroic coating on the outside of the glass meant to cut off the UV-C portion of their output. This seemed at first like a good thing but ended up causing the most catastrophic accident that this project saw. Because I am using an external triggering scheme, the trigger pulse would often arc across the conductive dichroic coating rather than couple into the glass and start the arc. Each time this happened the glass was left with a hairline imperfection. One night, while experimenting with ways to mitigate this problem, the tube violently exploded. Had the tube not been inside the pump head this would likely have been an injury-inducing accident and served as a fierce reminder as to just how much energy even a modest laser system like this one can deliver. The damage (minus the flashtube) was limited to minor scratching on the inside of the pump chamber and some small chips on the laser rod.

<figure>
<center><img src="/Resource/FLP_NdGlass/tube_explode_1.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Aftermath of the flashtube explosion.</b></figcaption>
</figure>

Needless to say, the IFP-800 left a bad taste of ozone and glass shrapnel in my mouth. I was lucky to find a different tube of domestic origin, and made in the last 20 years, on e-bay: the EM1019 from [Fenix Technology](https://www.fenixtechnology.com/showflash.html). This tube was almost exactly the same diameter and had almost exactly the same active length as the previous one, so I could use my pump head un-modified. The triggering was still an issue. The lamp needs about a 20kV pulse to trigger, and pulses at voltages that high have a habit of finding creative paths to ground, regardless of any attempts to insulate the pump head from the rest of the system. The solution that ended up working was wrapping a thin nickel wire around the active length of the tube and feeding it through a nylon bolt that I threaded through the side of the pump head. This kept the high voltage away from the metal body and eliminated the arcing problem. 

<figure>
<center><img src="/Resource/FLP_NdGlass/trigger_wire.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. New flashtube with trigger wire.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/FLP_NdGlass/trigger_bolt.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Trigger wire with insulating nylon bolt.</b></figcaption>
</figure>

## Trigger circuit
***
<br>
The trigger circuit consists of a 300V electrolytic cap meant for photoflash applications, connected via a high voltage SCR across an automotive ignition coil which is essentially an oil-filled transformer with an 80:1 turns ratio. This should give a pulse of 24kV on the output. This pulse is applied directly to the outside of the tube, where it is capacitively coupled to the electrodes creating a low-resistance path that the main cap bank can discharge across, resulting in a flash. The 300V cap is charged with an off-the-shelf high voltage flyback converter powered from a 12V wall wart. The whole trigger circuit is contained in a plastic box with a high-voltage connector and well-insulated alligator clip for connection to the trigger wire. 

<figure>
<center><img src="/Resource/FLP_NdGlass/trigger_circuit.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Trigger circuit contained in ABS enclosure.</b></figcaption>
</figure>

## Cap bank and charging circuit
***
<br>

The fluorescence lifetime of the Nd<sup>3+</sup> ion in the laser rod is 450&mu;s. This is the mean time between when the ion is moved to its excited state by absorbing a photon and when it emits another photon by falling back down to its ground state. In order to achieve a population inversion, a majority of the ions in the rod must be brought into the excited state during a given instance of time. There is some wiggle room here: the fluorescence lifetime of the ion is an average, and the pulse of light from the tube is not rectangular, there will be some "fade in" and "fade out" as it fires. What this means is that I can target a slightly longer pulse time than would be expected from the fluorescence lifetime of the ion, which makes building the circuit somewhat easier. In this case, I shot for a pulse time of 500&mu;s. To get a pulse this short, electrolytic capacitors can not be used, their internal inductance is too high. Polymer film caps are the gold standard for this application. I was lucky enough to find a crop of 8 UL30BF0290 750V 290&mu;F caps on e-bay. I placed 4 of these in parallel to get a capacitance of 1,160&mu;F and then placed two banks in series to get a maximum voltage rating of 1500V while halving the total capacitance down to 580&mu;F. This gives me a maximum energy storage of 652J. In operation, I only ever charged the bank to 1150V, which limited the maximum energy to 383J. At a pulse width of 500&mu;s this operates the tube at 22.44% of its "explosion energy" and should result in a lifetime of over 100,000 flashes.

<figure>
<center><img src="/Resource/FLP_NdGlass/cap_bank_2.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Main capacitor bank.</b></figcaption>
</figure>

In order to tune the system to get the correct pulse length a series inductor is added between the bank and the flashtube to lengthen the pulse time. The value of this inductor is found given the desired pulse length, the total energy in the bank, and some properties of the flashtube. Using this [calculator](https://www.fenixtechnology.com/fenixcalc.html), the needed inductor was found to be 47.89&mu;H. I made one using a large ferrite toroid and a few turns of solid-core 12AWG wire.

Charging this cap bank was another aspect of the project that went through several iterations. I tried at first to use an electrophoresis power supply. These are awesome as you can set them in constant voltage, current, or power modes, and they are the safest method available to most enthusiasts to charge high-voltage cap banks. Unfortunately, they are expensive and finding models that go much higher than 600V is difficult on the second-hand market. A more practical, but much less safe option, is to use a neon sign transformer (NST) hooked up to mains voltage and a high voltage diode to rectify the output. For these applications you need an old-school NST that's basically just a high turns ratio transformer, the new high-frequency digital NSTs will not work for cap chargers. Old-style NSTs can still be found on ebay, though in decreasing numbers. The smallest NST I could find was one that gave 3500V output at 120V input. To regulate the output down to the ~1.1kV I need for the cap bank I run the mains voltage through a variac before the input of the NST. This also gives some marginal increase in safety as I can slowly increase the voltage applied to the bank and creep up on the target voltage. The output of the NST is rectified through a single 5kV diode. To measure the voltage across the bank I used a 3kV analog panel meter.

<figure>
<center><img src="/Resource/FLP_NdGlass/NST.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Neon sign transformer used to charge main cap bank.</b></figcaption>
</figure>

## Optics
***
<br>
The optics for this laser were all purchased from a Chinese company called [master laser](https://master-laser.cn/). The mirrors (both full reflect and output coupler) I purchased are high-performance dichroic types tuned for the ~1064nm light an Nd:Glass laser produces. It is unlikely that at these modest output powers such mirrors are necessary or offer any real benefits. Sam Goldwasser's [laser blog](http://www.repairfaq.org/sam/lasercps.htm#cpstoc) states that a myriad of less than optimal mirrors have yielded success in these homebrew style systems, form polished pennies used for full reflects and stacks of microscope slides used for output couplers. Nonetheless, it is nice to eliminate the variable of the optics from an otherwise variable-ridden project. The mirrors are held in standard kinematic mirror mounts typically used in CO<sub>2</sub> laser cutters.

The focus lens was purchased from the same company, a similar wavelength-specific dichroic coated positive focal length lens with an inconveniently long focal length of 110mm. The lens was "mounted" even less precisely than the mirrors using a piece of wire and small alligator clip vice, which proved to be fine for what I needed. 

To align the cavity I used a commercial He:Ne laser mounted on a tripod and a piece of white paper. My mirrors are thick pieces of glass, partially reflective to the visible light of the alignment laser. Because of this there are multiple reflections off the front and back of the mirrors, which combined with the suboptimal mirror mounds makes aligning this laser very difficult, but with some screwing around it's possible to get it aligned well enough for a successful shot.

## Results
***
<br>
This "Zap-It" paper proved to be a very useful tool in determining how well the cavity was aligned, and the below comparison shows how sensitive the system is to this variable. 

<figure>
<center><img src="/Resource/FLP_NdGlass/weak_shot.JPG" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. The barely visible marks left by a poorly aligned cavity.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/FLP_NdGlass/strong_shot.JPG" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. The much stronger shot left by a better aligned cavity.</b></figcaption>
</figure>

It took the better part of 3 years to get this laser working, and there are still plenty of improvements that could be made. Maybe I will revisit this in the future, or maybe not. Regardless, the idea was more to explore the science, and the knowledge and skills that I gained in a diverse set of felids while working on this project is really the reason that we hobbyists take on things like this.

<figure>
<center><img src="/Resource/FLP_NdGlass/razor_shot_sparks.png" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Laser burning a hole in a razor blade.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/FLP_NdGlass/Razor Shot Clip.gif" style="max-width:100%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment fig_counter %}. Laser burning a hole in a razor blade.</b></figcaption>
</figure>
