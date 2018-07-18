---
layout: project
categories: PCB breakout
tags: usb breakout usb-breakout
permalink: /projects/pcb/:title:output_ext
project-category: PCB
hero-img: /images/projects/pcb/usbb/hero.jpg
featured-img: /images/projects/pcb/usbb/feature.png
schematic-img: /images/projects/pcb/usbb/sch.png
project-source: https://github.com/tahull/Eagle-projects/tree/master/usbbreakout
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left;"/>{% endif %}
There's not a whole lot to this project. A easy way to get a USB connection from one device to another is to chop up a USB cable, but I prefer a modular breadboard friendly component. The four small pins of a USB type B connector wont easily connect to a bread board, hence breakout board.

---
## Components
### Hardware
- USB type B jack
- Double sided PCB

### Software
- Eagle

---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" class="img-fluid"/>
{% endif %}

---
## Design
<img src="/images/projects/pcb/usbb/slide-show.gif" class="img-fluid"/>
### Hardware
A jack, a double sided PCB, a breakout component is born.

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
