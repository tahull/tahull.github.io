---
layout: project                               #file name: year-month-day-title.md
categories:                                   # category
tags:
permalink: /projects/:categories/:title:output_ext        # permalink if any
hero-img:
featured-img:                                 # featured image if any
schematic-img:
project-source:                               # sources
use-math: false
---


{% if page.featured-img %}
  <img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid mr-3" align="left"/>{% endif %}

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
