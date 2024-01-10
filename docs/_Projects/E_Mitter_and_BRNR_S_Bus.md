---
layout: page
title: "The E.Mitter and BRNR-S.bus"
description: "Low latency, high reliability control of unmanned vehicles over digital radio networks."
icon_img: "/Resource/E_Mitter_and_BRNR_S_Bus/E_Mitter_Ico.png"
dates: "(2019)"
page_order: 5
#permalink: /Nd-Glass/
---

# E.Mitter and Baud Rate Normalized Resilient S.Bus (BRNR-S.Bus) - low-latency, high-fidelity control of autonomous vehicles.   

## TL;DR:

A research group working in the defense sector developed a proprietary communications protocol to send packetized command and control data over an IP network for controlling semi-autonomous vehicles. The hardware layer was designed to be meshed, encrypted digital radio networks. The data would be sent from a PC based hand controller running a custom application with advanced path finding and targeting features, but on the vehicle end the data would be decoded into the standard Futaba S.Bus protocol, used on commercial radio control cars, to run the servos and motors. I designed a custom protocol, as well as the supporting hardware and firmware for an auxiliary communication subsystem to be used for critical vehicle control impervious to the latency and reliability concerns brought on the more complex (but more feature rich) IP based protocol. By running on a dedicated microcontroller, separate from the main operating system and CPU, low level control of the vehicle could be maintained even in the event of a higher level system failure. Although this protocol was designed to be used as a contingency control method, its low latency made it preferable when controlling the vehicles manually. 

To demonstrate the subsystem, I designed and built a custom hand controller to implement the custom protocol and successfully demonstrated command and control of a test vehicle. Using the IP based system the latency between moving an input on the controller and the motor responding was ~230ms, my custom protocol lowered this to ~80ms, under the 100ms target that the client deemed noticeable by a human operator. 


<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/E_Mitter_cad.png" style="max-width:70%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. CAD of the hand controller I designed and built to test BRNR-S.Bus</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/E_Mitter_Bench.png" style="max-width:70%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Assembled prototype of the hand controller designed for testing BRNR-S.Bus.</b></figcaption>
</figure>

# The Problem
***
<br>

MAVLink is a communications protocol developed for more advanced control of remote control vehicles. Specifically designed for drones, it can in principal be used to control any remote operated vehicle. MavNET is an extension of the MAVLink protocol designed for the defense sector. This proprietary protocol allows secure, multi-modal transmission of MAVLink packets to remote controlled combat vehicles. Multi-modal in this context means that the packets can be sent over an LTE cellular network, the iridium satellite cluster, or a line-of-sight digital radio mesh. This allows control of a vehicle from essentially anywhere in the world, with varying degrees of latency. A message sent via digital radio will have the least latency and one sent over the satellite cluster will have a very high latency. In most cases this latency is not an issue as the whole point of MAVLink is to send waypoint data, not manual motion control data like from a joystick. 

MAVNet requires a fairly complex stack to operate. Custom embedded computers are required on the vehicle to decode MAVLink packets and handle multiplexing them from the various sources, as as on the controller end to encode the waypoints and other command/control data for transmission to the vehicle. The hardware on both ends is running a full linux OS to handel the network switching and packet decoding. This complex stack means that reliability of the hardware and software is a concern, a software bug or OS crash could potentially brick the entire vehicle system. Due to the security sensitive nature of the application, having a vehicle stranded with no way to control or retrieve it was deemed too risky by the client. 

In addition, the MAVNet system adds a high degree of latency between sending a command to the vehicle and the motors being energized. For the waypoint system MAVNet was intended for this latency is not an issue, but when the vehicle is being controlled manually via a joystick over the meshed radio network the latency added by the encoding and decoding of MAVLink packets was noticeable to a human operator. 

# The Solution
***
<br>

To address these concerns I developed an auxiliary communications protocol designed to run along side MAVLink on septate low-level hardware to provide a robust, low-latency channel to control vehicles manually and recover them in the event higher level systems fail. I designed the protocol for use with meshed digital radios, which is it's most practical use case, but it could easily be extended for use with any of the other communication modes that MAVNet is compatible with. The digital radios used for this project provided an RS-232 serial line in addition to the normal IP network (The serial data was sent as UDP packets in the background, but on the hardware side it just looked like a normal RS-232 port), and this was chosen as the hardware layer of the new auxiliary protocol.  

The base of the vehicle under control is a commercial RC servo system, and the MAVLink packets eventually get decoded into a protocol called S.Bus which is standard on most RC receivers and servo controllers. For this reason I chose to base this new protocol on the S.Bus protocol so that minimal decoding was needed on the vehicle side. S.Bus has two quirks that make it difficult to use out of the box over an RS-232 channel: it operates at a non standard baud rate of 100,000 and uses inverted polarity (RS-232 treats high voltage as a "0"). In addition, S.Bus has no built in means of checking for data integrity, for a protocol meant to serve as a mission critical backup, this was not ideal. 

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/S_Bus.svg" style="max-width:50%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. "Stock" S.Bus data packet format.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/BRNR-S_Bus.svg" style="max-width:50%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Modified "BRNR"-S.Bus data packet format.</b></figcaption>
</figure>


The "stock" S.Bus protocol is seen in Fig 2, each pair of 8 bit data frames define a servo channel and power so each S.Bus packet can update the states of 11 servos at a time. The modified S.Bus protocol shown in Fig 3 removes the "Flags" frame (which was not being used for this application) and adds a CRC-8 checksum byte to the end. This new protocol is called Baud Rate Normalized Resilient S.Bus or BRNR-S.Bus. A full system view is shown in Fig 4, the microcontroller reads joystick positions and encodes the servo channel/value pairs based on a had coded configuration file. One the packet is formed the CRC checksum is performed and appended to the end. The BRNR-S.Bus packet is sent over the RS-232 channel on the digital radio network. Once received, the microcontroller on the vehicle side checks the CRC checksum to make sure no data was lost in transit, adds a dummy flag frame to make the packet a valid S.Bus packet again and re-transmits it to the servo controller at 100,000 baud with inverted polarity.

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/BRNR-System.svg" style="max-width:80%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Full system overview showing BRNR-S.Bus controlling the vehicle system.</b></figcaption>
</figure>

In order to test and demonstrate this new protocol, I designed a prototype hand controller (dubbed the E.Mitter). This controller was built into a 3D printed shell with off the shelf electrodes and buttons for configurable vehicle control. The large LCD screen in the center is designed to be connected to a PC running a custom version of Q ground control for use with MAVNet. The small OLED screen on the right side is designed to be controlled by the auxillary microcontroller and provide critical information in the event the PC crashes or otherwise malfunctions. 
The internals of the E.Mitter are shown in Fig 5. The hand soldered breadboard in the top left contains the microcontroller, as well as some level shifting electronics for the RS-232 to TTL conversion. The RS-232 signal is output from the LEMO connecter on the bottom of the controller. 

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/E-Mitter_Inside.png" style="max-width:80%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Inside of the E.Mitter showing the LCD screen, batteries, joysticks, and the breadboard that contains the microcontroller.</b></figcaption>
</figure>

An MK66FX1M0VMD18 MCU from freescale was integrated into the main vehicle control PCB to handle the decoding of BRNR-S.Bus packets, this microcontroller ran custom firmware I developed in C++. The high performance overhead of this auxiliary microcontroller allowed for the possibility to parse more advanced data streams in the future.


# Results
***
<br>

The BRNR-S.Bus protocol was successfully integrated into the main vehicle control PCB for the project using an MK66FX1M0VMD18 MCU and my C++ firmware. My prototype hand controller, though not designed for use in the final product, allowed testing and debugging while waiting for other groups to deliver the official hand controller. In addition to providing a reliable method of backup communication and control the BRNR-S.Bus system is also designed to improve the latency of vehicle control. I designed a simple test procedure to determine how much latency exists between the movement of the joysticks and the actuation of the servo motor. By connecting one channel of an oscilloscope to the analog output of the joystick on the hand controller and another channel to the PWM output of the servo connector on the vehicle side, this latency could be measured.

Shown in Fig 6 is the latency of a standard RC control system consisting of a commercial transmitter and receiver, the latency between moving the joystick and the PWM signal to the motor updating to reflect that is about 100ms, this latency will not be perceivable to a human operator.

Fig 7 shows the latency measured in the same way when the data is sent via MAVLink packets over the custom MAVNet protocol running on meshed digital radios. This latency of 230ms will be noticeable to the operator as a lag between telling the vehicle to move and the vehicle actually moving.

Fig 8 shows the BRNR-S.Bus protocol sent via the RS-232 channel of the same digital radio network, this protocol beats even the traditional RC network with only 82ms of latency.

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/futaba1.PNG" style="max-width:80%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Latency of traditional RC control system.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/Mavlink.PNG" style="max-width:80%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Latency of MAVNet system.</b></figcaption>
</figure>

<figure>
<center><img src="/Resource/E_Mitter_and_BRNR_S_Bus/BRNR-S.Bus_Timing.PNG" style="max-width:80%; min-width:300px; height:auto" class="center"></center>
<figcaption align="center"><b>Fig {% increment BRNR-fig_counter %}. Latency of BRNR-S.Bus system.</b></figcaption>
</figure>