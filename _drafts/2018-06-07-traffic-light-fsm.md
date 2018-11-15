---
layout: project                               
categories: [PIC Projects]                                 
tags: trafficlights FSM
permalink: /projects/pic/:title:output_ext     
hero-img:
featured-img:                                 
schematic-img:
project-source: https://github.com/tahull/PIC-projects/tree/master/Traffic_Lights.X
use-math: false
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid mr-3" align="left"/>{% endif %}
A Traffic light simulation is a classic text book problem used to demonstrate a state machine.

Its  common to see a state machine implemented as "if" or "switch" case statement. Another approach is to use a struct. using a stuct condenses the state machine logic quite a lot and makes debugging or adjusting states easy.

<img src="/images/projects/pic/traffic-lights/state-graph.png" alt="image of state graph" title="state graph" class="img-fluid" align="left"/>
<img src="/images/projects/pic/traffic-lights/state-table.png" alt="image of state table" title="state table" class="img-fluid" align="left"/>



---
## Components
### Hardware

### Software

---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" alt="image of wiring diagram" title="wiring diagram" class="img-fluid"/>
{% endif %}

---
## Design
### Hardware

### Software

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
