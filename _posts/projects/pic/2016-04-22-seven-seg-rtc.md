---
layout: project
categories: PIC rtc
tags: rtc multiplex seven-seg-display pov display clock
project-category: PIC
permalink: /projects/pic/:title:output_ext
featured-img: /images/projects/pic/rtc/cover.jpg
project-source: https://github.com/tah83/PIC-projects/tree/master/Seven_Seg_RTC.X
---

<img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left; max-width:15rem;"/>
This is a simple RTC project displaying numbers to multiplexed seven segment displays. Multiplexing combined with persistence of vision takes advantage of fast processing and slow human eyes, while using a minimal amount of signal lines to control multiple displays. At any point in time only one display is active, but cycling through the displays fast enough, gives the appearance all displays are active.

---
## Components
### Hardware
- 4-digit 7-seg common cathode display
- 4x 4.7k ohm resistors
- 7x 330 ohm resistors
- 4x S8050 NPN transistors
- 3x push buttons
- PIC16f1937 micro controller

### Software
- Mplabx IDE 3.26
- XC8 1.37 compiler
- MCC code configurator

---
## Schematic
Schematic created with easyEDA
<img src="/images/projects/pic/rtc/sch.png" class="img-fluid"/>

---
## Design
### Hardware
#### Multiplexing 7-seg display
With a single display I would only need seven IO pins from the micro controller to control the LED segments, with four digit seven segment display I would need to use 28 pins to control all 28 segments. With more displays the pin requirement adds up fast. Multiplexing makes the pins and wires requirement more manageable. One set of seven pins is connected to the seven segments of all the displays, and an additional four pins needed to select which display is active. At any time, only one display is activated. A display is activated, the LEDs in that segment are lit to display a desired number, then that display is de-activated, the next display is activated. This cycle happens around 50 times a second to create the appearance of a non-flickering consistent bar of lighted numbers.
On this four digit 7-seg display the first digit only has segments 'b' and 'd' so I combined the upper left dot, upper right dot and colon to be controlled along with the first digit, this saves on have to use another transistor, and additional pins to control these segments.

#### Driving a 7-seg display
<img src="/images/projects/pic/rtc/npn-cir.png" class="img-fluid" style="float:right; max-width:15rem;"/>
A single common cathode seven segment display is activated by holding the common cathode pin low, and the desired segment pins high. With a single display the common cathode pin could be connected to ground. The segment pins would source the current to light the LED's and current would flow to ground. For this project I needed to be able to switch displays, so the common cathode pins need to be controlled by the micro controller. If all segment pins are active the cathode pin would need to sink 7x current sources from the seven segment pins, which could be too much current flowing through one pin for the micro controller. This is where the transistors come in. The NPN transistor will act as a switch, when the transistor is turned on, it will act as a short between the collector and emitter with the current at the emitter Ie = Ic+Ib. A resistor is needed on the base to limit current flowing to the base of the transistor.

### Software
For this project I used the MCC code configurator plugin, this made implementation easy and fast.  Normally I'd spend more time reading the datasheet for register names and configurations, but thanks to the configurator I quickly setup timers, and interrupts. This configuration consists of three timers, one used to create a non blocking button de-bounce, one for creating an approximate 5 ms interrupt used for cycling the seven segment displays, and another timer used for the real time clock.

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">Seven Seg RTC</a>
{% endif %}
