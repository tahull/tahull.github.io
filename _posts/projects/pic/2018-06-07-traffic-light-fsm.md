---
layout: project                               
categories: [PIC Projects]                                 
tags: [traffic light simulation,LEDs, FSM, Moore machine]
permalink: /projects/pic/:title:output_ext     
hero-img: /images/projects/pic/traffic-lights/hero.jpg
featured-img: /images/projects/pic/traffic-lights/feature.png
schematic-img: /images/projects/pic/traffic-lights/sch.svg
project-source: https://github.com/tahull/PIC-projects/tree/master/Traffic_Lights.X
use-math: false
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid mr-3" align="left"/>{% endif %}
Traffic light simulation is a classic text book problem used to demonstrate a finite state machine. A FSM is a system with finite states, finite inputs, finite outputs. It has a known number of states, inputs, and outputs. You can join team Mealy or team Moore, sometimes it matter, either way everybody wins, and you can switch teams in most cases. The state machine implementation in this project is an example of a Moore machine. Output depends on the current state and the next state depends on current state and input.
```c
output = output(current_state)
next_state = state_transition(current_state,input)
```

Opposed to Mealy model where output depends on current state and input and next state depends on current state and input.
```c
output = output(current_state,input)
```

This traffic light simulation will simulate two one-way streets. One street has priority (north to south) and will stay green unless there's a reason to switch.

<div class="embed-responsive embed-responsive-16by9 col-md-10 col-lg-7">
<iframe class="embed-responsive-item" width="560" height="315" src="https://www.youtube.com/embed/-6fCYKG5Xbk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---
## Components
### Hardware
- PIC16f1829
  - Any micriocontroller will do.
- 6x LED's
  - 2x Red 2x Yellow 2x Green to simulate traffic lights.
- 6x 330 ohm resistors
  - Current limiting resistors for LED's.
- 4.7k ohm resistors
  - Pull down resistors for switches.
- 2x Push button switches
  - Simulate traffic sensor.
- Breadboard
- Jumper Wires
- Popsicle sticks
- Bottle caps
- Card board

### Software
- MPLABX
- MCC Microchip Code Configurator
- EasyEDA

---
## Schematic
{% if page.schematic-img %}
  <img src="{{ page.schematic-img }}" alt="image of wiring diagram" title="wiring diagram" class="img-fluid"/>
{% endif %}

---
## Design
<div class="row">
  <div class="col" markdown="1">
  <img src="/images/projects/pic/traffic-lights/state-graph.png" alt="image of state graph" title="state graph" class="img-fluid"/>
  </div>
  <div class="col" markdown="1">
  What are the states.
  - state0: green on north to south, red on east to west
  - state1: yellow on north to south, red on east to west
  - state3: red on north to south, green on east to west
  - state4: red on north to south, yellow on east to west

  What causes state transitions
  - No traffic
  - Traffic north bound
  - Traffic east bound
  - Traffic east and north bound
  Four states, and four transition.
  </div>
</div>

<div class="row">
  <div class="col" markdown="1">
  With the state graph done, its easy to build the state table. Looking at state0 (S0) if there's no input (no traffic) then the next state is S0 (main light stays green). If there's east bound traffic (0b10) then the next state will be S1 (north to south light changes to yellow). the Full transition will be S0 -> S1 -> S2 -> S2 and stay in S2 until there's no east bound traffic or if north bound traffic arrives.
  </div>
  <div class="col" markdown="1">
 <img src="/images/projects/pic/traffic-lights/state-table.png" alt="image of state table" title="state table" class="img-fluid float-right"/>
 </div>
</div>

### Software
Its  common to see a state machine implemented as "if" or "switch" case statement. Another approach is to use a struct. using a stuct condenses the state machine logic quite a lot and makes debugging or adjusting states easy.

The state table created earlier can nearly be copy pasted to the state table for the program.
```c
const struct state traffic_controller[4] = {
    //output,           delay,  next state
    //                           00  01  10  11
    {NS_GREEN|EW_RED,   5,      {S0, S0, S1, S1}},  // S0
    {NS_YELLOW|EW_RED,  1,      {S2, S2, S2, S2}},  // S1
    {NS_RED|EW_GREEN,   5,      {S3, S3, S2, S3}},  // S2
    {NS_RED|EW_YELLOW,  1,      {S0, S0, S0, S0}}   // S3
};  
```
The state machine can be run in the main while loop with four lines
- Set output
- Wait delay
- Read input
- Set next state

```c
while (1)
{
    //set output. LED's connected to port C
    LATC = traffic_controller[state].output;
    //wait state
    delay_s(traffic_controller[state].delay);
    //read buttons
    input = (uint8_t)(IO_RB5_PORT|(IO_RB6_PORT<<1u));
    //set next state
    state = traffic_controller[state].next[input];
}
```

### Hardware
Card board, popsicle sticks, and hot glue.

<img src="/images/projects/pic/traffic-lights/slide-show.gif" alt="image of project stages" title="project slide show" class="img-fluid"/>

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}

## Resource
Introduction to ARM Cortex-M Microcontrollers (Jonathan W. Valvano)
