---
layout: post
categories: tips&tricks
tags: pcb-transfer pcb-printout
permalink: /:categories/:year/:month/:title:output_ext
---


If you're like me and want to get multiple PCB layers on one printout rather than one per page. Here's a simple method to achieve that.

<iframe width="560" height="315" src="https://www.youtube.com/embed/U1sPeEALW9o" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


---
## Procedure
When exporting to an image file, its possible that the dimensions may be changed which will give an inaccurate PCB. Also image printing default settings seem to like the "fit to page" option. Whatever image editing software is used, you may need to check that it doesn't modify the size of the image when printing.

I'm using Eagle Cad and MS Paint. Eagle allows exporting to an image with a set dpi. The dots per inch needs to be consistent between the different images. When exporting as an image, eagle includes extra information like pad names, signal trace names which we don't need in the image, and may cause problems with the PCB, so its a good idea to turn these off. To disable those in eagle select the Options -> Set... go to the Misc tab and uncheck Display pad names, Display Signal names on pads, and Display signal names on traces . When the PCB is ready to be printed:

- Export to image. From Eagle file menu File -> Export -> image. Or use a script to automate the process.

```
ratsnest
display none
display bottom dimension pads vias
export image bottom.png monochrome 600;
display Top -Bottom
export image top.png monochrome 600;
display none
display all
ripup @;
export image reference.png monochrome 600;
```

 Save the above text to file with .scr extension in the /eagle/scr directory. The script can be run with "script 'script name'" from the command line in eagle. Or File ->Execute Script... the images will be exported to the active eagle project folder, or a location can be specified for example:
 export image C:/Users/'username'/Desktop/bottom.png monochrome 600;
 Edit the images in MS Paint. I needed my top layer inverted so I used the "flip horizontal" option.
 Printing the image from MS Paint. Open File->Print->Page setup and set the scaling to "100% normal size"
