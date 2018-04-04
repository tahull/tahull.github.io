---
layout: project                               #file name: year-month-day-title.md
categories: PCB breakout                                # category
tags: usb "usb breakout"
permalink: /projects/pcb/:title:output_ext        # permalink if any
project-category: PCB                          # project type/technology used
featured-img: /images/projects/pcb/usbb/cover.gif                                # featured image if any
schematic-img:
project-source: https://github.com/tah83/Eagle-projects/tree/master/usbbreakout                              # sources
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left; max-width:15rem;"/>{% endif %}
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
### Hardware
A jack, a double sided PCB, a breakout component is born.

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
