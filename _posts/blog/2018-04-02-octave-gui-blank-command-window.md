---
layout: post
categories: [linux sucks]
tags: [octave, ubuntu 16.04, linux]
permalink: /blog/:year/:month/:title
redirect_from:
  - /linux%20sucks/2018/04/octave-gui-blank-command-window.html
hero-img: /images/blog/octave/blank-command.png
featured-img: /images/blog/octave/feature.png
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" alt="image of {{ page.title }}" title = "{{ page.title }}" class="img-fluid mr-3" style="float:left;"/>{% endif %}
Blank octave command window :( This is one of those annoying errors where part of the functionality is crippled, and you're likely going to spend hours hunting down a fix, or applying updates, un-install, re-install, "did you try restarting it," downgrade to previous version, upgrade to the next version, google it. Maybe if you restart if for the 20th time, and look at it just the right way it will work.

## The Problem
After upgrading Octave from 4.0.0 to 4.2.1 on Ubuntu 16.04. The Octave command window went blank. I could still type in commands, and have it show in command history, but the command window itself wouldn't show what was happening.

Initially I tried changing some settings/ reset some defaults. Searched for similar issues and solutions. purged and re-installed octave (not a fix).

Some results I found suggested its a configuration problem between opengl and qt. I'm guessing the issue is something to do with old settings or name changes not being updated from the previous versions qt-settings file in Octave's config.

## The Fix
Delete the qt-settings or entire octave config folder, then octave will generate a new config and present you with a welcome screen the next time its opened.

Remove octave config folder.
```bash
rm -r /home/$USER/.config/octave
```

<img src="/images/blog/octave/success.png" alt="image of octave welcome screen" title = "octave welcome screen" class="img-fluid"/>

And command window is back.

<img src="/images/blog/octave/success2.png" alt="image of working octave command window" title = "working octave command window" class="img-fluid"/>
