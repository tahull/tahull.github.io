---
layout: post
categories: responsive-images                                 
tags: grunt responsive-images image-optimizing
permalink: /:categories/:year/:month/:title:output_ext                             
hero-img:
featured-img:                                 
schematic-img:
use-math: false
---

At first I assumed jekyll would be doing some spiffy automagical stuff when "building" my site. I assumed I could throw in pictures of any size and quality and jekyll would resize and optimize the image to fit its use cases. So without checking image pixel dimensions and file sizes, some of my pages became quit slow at loading with page load of multi megabytes. Lesson learned, now I needed to learn a little about optimizing images.
Udacity offers a free course called responsive Images.

The gist of responsive images is
- Don't use images unless you need to
  - use svgs or CSS tricks instead
- Use srcset and let the browser pick the best option
- resize and optimize Images


## Resources
- google pagespeed insights
- Udacity - responsive images
