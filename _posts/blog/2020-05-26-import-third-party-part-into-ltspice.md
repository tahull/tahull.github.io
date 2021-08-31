---
layout: post
categories: [Tips And Tricks]
tags: [LTspice]
permalink: /blog/:year/:month/:title
---

LTspice is FREE, it's good too. SPICE netlist is script-like commands for circuit simulations. With LTspice the circuit can be setup graphically with the SPICE netlist living in the background.

LTspice has complete models for Analog Devices, Linear Tech parts, but if you want to simulate a part that is not in the parts list, you can do that too. LTspice has some blank graphical models allowing a simulation model to be attached to it. It's possible to simulate non-LT or AD parts that have a SPICE .subckt or .model.

## Example with a lm358 op-amp
Parts manufactures often supply simulation models for their parts, for a [TI LM358](https://www.ti.com/product/LM358/toolssoftware) there's a PSPICE model under `Design Tools & Simulations`. Or [OnSemi](https://www.onsemi.com/products/amplifiers-comparators/operational-amplifiers-op-amps/lm358) has their version under `Technical Documentation & Design Resources`

LTSpice has a generic opamp `opamp2` which has five ports, in+, in-, v+, v-, and out. This should match the same order with the .subckt declaration in the simulation file.

OnSemi LM358 SPICE MODEL.MOD file
```
* PINOUT ORDER +IN -IN +V -V OUT
* PINOUT ORDER  1   2   3  4  5
.SUBCKT LM358 1 2 3 4 5
```
TI LMx58_LM2904.CIR file
```
.subckt LMX58_LM2904 IN+ IN- VCC VEE OUT
```
Don't get tripped up by the file extension, as long as its a SPICE or PSPICE simulation model, it should work.

In LTSpice add the generic opamp2  
![generic opamp2 in ltspice](/images/blog/ltspice/opamp2.jpg){:class="img-fluid "}  

Right click the part, and change the component attribute value  
![part attribute](/images/blog/ltspice/opamp2-attribute.jpg){:class="img-fluid "}  

Change the value to the subckt name  
Im using the TI's PSPICE file: LMx58_LM2904.CIR. Open the file in a text editor and find the .subckt name.  
```
.subckt LMX58_LM2904 IN+ IN- VCC VEE OUT
```
![edited part attribute](/images/blog/ltspice/opamp2-attribute-change.jpg){:class="img-fluid "}  

Add spice command .include 'filename'  
![include command](/images/blog/ltspice/command-include.jpg){:class="img-fluid "}

And now LTSpice can run the simulation for this lm358  
![image of simulation running](/images/blog/ltspice/final-run.jpg){:class="img-fluid "}
