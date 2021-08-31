---
layout: project
categories: [Tips And Tricks]
tags: [Diode Tricks, Hacks, Sound Card Oscilloscope]
permalink: /projects/tips-tricks/:title:output_ext
hero-img: /images/projects/tips-tricks/o-scope/hero.jpg
featured-img: /images/projects/tips-tricks/o-scope/feature.jpg  # featured image if any
schematic-img: /images/projects/tips-tricks/o-scope/sch.png  
project-source:                               # sources
---

## Intro
{% if page.featured-img %}
  <img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid mr-3" align="left"/>{% endif %}
An oscilloscope is a valuable piece of test equipment that can help in analyzing signals. It helps to be able to see what is happening with a signal in terms of voltage and time.
A sound card's mic or line-in combined with oscilloscope software makes for a cheap piece of test equipment, but with some major shortcomings. A sound card is designed to work with frequency's in the audible range 20-20khz, sound cards do oversampling at 44khz or 96khz. Although to get enough sample points to get something view-able/useful with a sound card, the source signal will probably need to be around 20khz and under. Sound card's mic and line-in are made to connect with audio equipment, the interface isn't designed for high voltages, so to avoid damaging the interface , the signal needs to be conditioned. Oscilloscope software probably wont help with viewing voltage levels of a signal.
So... there's software that can allow a sound card to be used as an oscilloscope, however it's only useful for small signals and low frequencies.

---
## Parts
- 2x 500k audio pot
- 4x 1N4148 diode
- 2x 4.7k resistor
- Audio cable

### Software
<a href="https://www.zeitnitz.eu/scope_en">Soundcard Oscilloscope</a>

<img src="/images/projects/tips-tricks/o-scope/soundcard-scope-sw.png" alt="image of soundcard oscilloscope software displaying square wave" title="soundcard oscilloscope software" class="img-fluid"/>

And here's some more sound card oscilloscope options:     
<a href="http://www.zen22142.zen.co.uk/Prac/winscope.htm">Winscope</a>   
<a href="http://www.zelscope.com/index.html">Zelscope</a>    

---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" alt="image of wiring diagram" title="wiring diagram" class="img-fluid"/>
{% endif %}

---
## Design
### Hardware
This hardware design is adapted from
<a href="http://homediyelectronics.com/projects/howtomakeafreesoundcardpcoscilloscope/">Steve Garratt</a> and his sound card oscilloscope. This is a simple hardware design that will help to keep a test signal in a safe range for the sound card.  A diode limiter circuit will clip the voltage at +.6v to -.6v. A notable word here is "clip" this signal in this simulation run is clipped, the signal above .6v and below -.6v is cut off.

<img src="/images/projects/tips-tricks/o-scope/diode-limiter1.png" alt="image of diode clamp circuit" title="lt spice diode clamp circuit" class="img-fluid"/>

This is where the potentiometer comes in, to drop the signal voltage down to a range under the 600mv to -600mv.

<img src="/images/projects/tips-tricks/o-scope/diode-limiter2.png" alt="image of diode clamp circuit 2" title="lt spice diode clamp circuit 2" class="img-fluid"/>
