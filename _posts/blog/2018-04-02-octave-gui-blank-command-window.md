---
layout: post
categories: linux-sucks
tags: octave linux ubuntu16.04
permalink: /:categories/:year/:month/:title:output_ext
hero-img: /images/blog/octave/blank-command.png
featured-img: /images/blog/octave/feature.png
---

{% if page.featured-img %}
  <img src="{{ page.featured-img }}" class="img-fluid mr-3" style="float:left;"/>{% endif %}
Blank octave command window :( This is one of those annoying errors where part of the functionality is crippled, and you're likely going to spend hours hunting down a fix, or applying updates, un-install, re-install, "did you try restarting it," downgrade to previous version, upgrade to the next version, google it. Maybe if you restart if for the 20th time, and look at it just the right way it will work.

After applying an update for graphics driver, and Octave on Ubuntu 16.04. Octave updated from 4.0.0 to 4.2.1, the octave command window went blank. I could still type in commands, and have it show in command history, but the command window itself wouldn't show what was happening.

Initially I tried changing some settings/ reset some defaults. Searched for similar issues and solutions. purged and re-installed octave (not a fix).

Some results I found suggested its a configuration problem between opengl and qt. I'm guessing the issue is something to do with old settings or key-value-pairs not being updated from the previous versions qt-settings file, a name change, a new key, an old key removed, who knows. Octave or the package manager doesn't handle the update migration well or cant overwrite the old setting file.

## TL;DR
Delete the qt-settings or entire octave folder, then octave will generate a new config and present you with a welcome screen the next time its opened.

Remove octave config folder.
```console
rm -r /home/$USER/.config/octave
```

<img src="/images/blog/octave/success.png" class="img-fluid"/>

And command window is back.

<img src="/images/blog/octave/success2.png" class="img-fluid"/>
