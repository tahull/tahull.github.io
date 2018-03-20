---
layout: project                               #file name: year-month-day-title.md
categories:                                   # category
permalink: /projects/:title:output_ext        # permalink if any
project-category:                          # project type/technology used
featured-img:                                 # featured image if any
schematic-img:
project-source:                               # sources
---


<div class="projects-scroll" id="intro" markdown="1">
---
## Introduction
{% if page.featured-img %}
  <img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left; max-width:15rem;"/>
{% endif %}


</div>

<div class="projects-scroll" id="parts" markdown="1">
---
## Parts:

### Software:

</div>

<div class="projects-scroll" id="schematic" markdown="1">
---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" class="img-fluid"/>
{% endif %}

</div>

<div class="projects-scroll" id="design" markdown="1">
---
## Hardware Design

### Software Design

</div>

<div class="projects-scroll" id="sources" markdown="1">
---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
</div>
