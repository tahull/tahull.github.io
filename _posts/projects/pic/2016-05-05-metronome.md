---
layout: project                               #file name: year-month-day-title.md
categories: PIC metronome                                   # category
tags: metronome
permalink: /projects/pic/:title:output_ext        # permalink if any
project-category: PIC                            # project type/technology used
hero-img:
featured-img:                                 # featured image if any
project-source: https://github.com/tahull/PIC-projects/tree/master/Metronome.X     # sources
schematic-img: /images/projects/pic/metronome/sch.png
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left; max-width:15rem;"/>
{% endif %}
 A metronome is a device used by musicians to assist with proper timing. A metronome produces regular ticks at a user selected beats per minute. The beats per minute for this project will vary between 30 to 140 (this range can be easily changed).

---
## Components
### Hardware
- PIC18f4320
- Piezo PKM22EP sound component
- 1x 330 ohm resistor
- 2x 1k ohm resistor
- 2x 10k ohm resistor
- 1x NPN transistor
- hd44780 LCD

### Software
- Mplabx
- XC8 c compiler

---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" class="img-fluid"/>
{% endif %}

---
## Design
### Hardware
<img src="/images/projects/pic/metronome/simple-piezo.png" class="img-fluid mr-3" style="float:right; max-width:15rem;"/>
The hardware is minimal, a piezo speaker for making noise, an LED, LCD, some push buttons, and some resistors.Generating sound can be as simple as connecting a piezo buzzer between a micro controller pin and supply rails. I chose to use a transistor so the supplied voltage to the buzzer is the system rails. The micro controller supplies the frequency.
   A LED is used in this project to add a visual metronome, with the LED flashing with each beep.

### Software
The software consists of polling on the push buttons and generating beeps using the micro controllers PWM. Information is displayed to a LCD.
Its important to be able to get the values needed for generating the timing.
- Guess and check:
  - Using a simulator
- Calculate it:
  - With 8-bit pic micro controllers we know the instruction clock is 1/4 the clock

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
