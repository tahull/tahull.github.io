---
layout: project       #file name: year-month-day-title.md
categories: [Lego Projects]
tags: [Flood Fill, Lego Mindstorm, Micromouse]
permalink: /projects/lego/:title:output_ext            # permalink if any
hero-img: /images/projects/lego/mouse/hero.jpg
featured-img: /images/projects/lego/mouse/feature.png        # featured image if any
project-source: https://github.com/tahull/LegoMicroMouse      # sources
---

## Intro
<img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid" class="mr-3" align="left"/>
This was an in-class project I had. The focus in this project was meant to be more on the flood fill algorithm than hardware design, so the Lego Mindstorm platform was provided in the class. The Lego Mindstorm platform is well suited for quickly creating hardware but with limited component choices. Simply choose from the available sensors, actuators, motors and snap them together in the desired configuration.
A micro-mouse is a robotic maze solver, like its flesh and blood counterpart a mouse or rat in a maze. Instead of looking for food in the maze the robotic version searches for the goal, the center of the maze. A typical maze would be 16x16, but for the purposes of saving space, the class competition was held on a 5x5 maze. This project could be extended to a 16x16 maze by changing the #define MAZE_WIDTH and MAZE_HEIGHT to 16 and 16 respectively.

<div class="embed-responsive embed-responsive-16by9 col-md-10 col-lg-7">
  <iframe class="embed-responsive-item" width="560" height="315" src="https://www.youtube.com/embed/lDM9-iq9xZ8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>
---
## Components
### Parts
- 3x Lego distance sensor
- 2x Lego servo motors
- Lego NXT brick
- Various  Lego connectors to hold it all together

### Software
- NXC (not exactly C): A c-like programming language.


---
## Design
### Hardware
The goal of the hardware design is to allow the bot to move easily about in the maze. Easier said than done, I found that some of the servo motors weren't quite in sync and the bot would drift to one side. Using the OnFwdSync() from the nxc api can compensate somewhat for the drift by reading the rotation count from the servo motor encoders and attempting to keep the motors in sync. However, I noticed there was still some drift depending on the motor pairing. Luckily, in this class, there were enough Mindstorm kits for everyone and then some, thanks to the Chico State Computer Science department. I was able to find a motor pairing that closely operated at the same speed and power. Three distance Sensors were used to help determine if the bot was drifting to close to a wall, to far from a wall, if a wall was present in the maze and where. The distance sensors pointed forward, left and right.  For the rear of the bot I tried a few things- a sled, a free moving third wheel, and a ball (not part of the Mindstorm kit). The sled design occasionally got hung up on the board during turns, the free moving third wheel caused the bot to go in wrong directions during reverse maneuvers, the loosely held ball allowed for the least restrictive or interfering movements.

### Software
The job of the software is to tell the bot how to move in the maze, and to find the shortest path to the goal in the maze. The bot needed constant adjustments while moving in the maze; a 90 degree turn wasn't always a perfect 90 degree turn. The bot needed to look at its surrounding and correct itself. Part of the code, more than initially anticipated, consisted of movement control and corrections; the rest was the implementation of the flood fill algorithm, loosely detailed below in pseudo code. The bot will attempt to move to the next cell with the significant value. The bot will move from high to low values, with the goal being 0. If its blocked by a wall or there is no next move it will re-flood the maze to get new values. As the algorithm name suggests, the maze is "flooded" with weighted values. Think of a bucket of water being dumped at one point in the maze. If the goal is the center, the bucket of water is dumped at the center. The center will have the most significant weighted value 0. As time increases, in this analogy, the water will hit the next accessible cell or cells (depending on known walls), those cells will be given a value of 1. Tick, the next cells the water hits get a value of 2, and so on. As the water traverses the maze, a shortest path to the goal will emerge. In that analogy, we would have an overhead view of the maze, we could see all the walls, but the bot doesn't. The bot only knows the walls around itself and the walls its already seen. So as the bot traverses the maze, discovering where the walls are, it needs to constantly re-flood the maze with values.

pseudo code:
```
main(){
    loop forever{
        if bot is not at the target
        check the cell walls in the current cell
        get the next best direction to move in
        if no good direction,
            flood the maze to get new values
        move to next direction and update bots position
        if bot is at the target,
            make some noise set the new target to be the bot starting location
    end loop
end function
/////////////
get next direction(){
    check the neighbor cells for lower distance value
    if we have two cells with the same value
        choose in this order (North, East, South West)
end function
/////////////////
flood maze(){
    reset maze values so we can re-flood and update the values
    put the first cell ( the target cell) to examine on the stack and set its significant value to 0
    while ( we have cells on the stack){
        check if there is a detected walls to the neighbor cells North, East, South and West
        if there is no wall and the neighbor cell hasn't already been set
            put the cell on the stack and set the cells distance value to the current cells value + 1
    end loop
end function
```

<img align="right" src="/images/projects/lego/mouse/lcd-display.png" alt="image of lego brick lcd displaying micromouse maze" title="micromouse maze" class="img-fluid"/>
To help with debugging and making sure the bot was doing what I thought it should be doing, I made use of the LCD display on the brick. I used the api to draw lines for seen walls and displaying their cell values. This is the display after the maze has been explored (not the same maze configuration as in the first video). The starting position is noted with "S" the bots current position noted with "B" and the goal at the center has a value of 0.

---
## Source files
{% if page.project-source %}
  <a href="{{ page.project-source }}">{{page.title}}</a>
{% endif %}
