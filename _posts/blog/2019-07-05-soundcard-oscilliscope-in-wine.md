---
layout: post
categories: [Linux Sucks]
tags: [Wine, Linux, Sound Card Oscilloscope, Ubuntu 18.04]
permalink: /blog/:year/:month/:title
---

A sound card oscilloscope application is useful for viewing and analyzing low frequency signals (20Hz - 20KHz). I wanted to do some work with audio signals, and redo and improve a college project of mine.
There's probably a native Linux sound card oscilloscope application that would do everything I wanted, and bonus points if it works out-of-the-box. Linux as a desktop sucks, it always seems to bite me when I just want to use it, install a tool and use it. Some assembly required. Understanding that, Linux as a desktop is awesome, there's lots of developer tools, different distributions , active communities, lots of free and open source software, the price is right, and if something doesn't work and you have the time and motivation, there's a solution.

At first I tried `xoscope` but couldn't get it to work with my sound card's mic input. Failing with `xoscope` I tried a familiar built-for-windows application instead, [https://www.zeitnitz.eu/scope_en](https://www.zeitnitz.eu/scope_en). I had used this program before on Ubuntu, and didn't recall having issues, or if I did, I forgot what the fix was. Here's a self reminder to document these sorts of things.

Some of the features of `Soundcard Oscilloscope` that I wanted:
1. Oscilloscope display
1. Frequency display
1. Set a trigger level
1. Trigger mode "single"
1. Measure frequency
1. Log to file (as CSV)

Here's how I prepared Wine: [Setup A Wineprefix On Ubuntu 18.04](/blog/2019/06/setup-a-wineprefix-on-ubuntu-18.04)

<div class="row">
  <div class="col-md">
    <figure>
       <img src="/images/blog/scope/osci.jpg"
          alt="Image of oscilloscope with frequency" class="img-fluid"/>
       <figcaption>Oscilloscope display</figcaption>
    </figure>
  </div>
  <div class="col-md">
    <figure>
       <img src="/images/blog/scope/freq.jpg"
          alt="Image of frequency display" class="img-fluid"/>
       <figcaption>Frequency display</figcaption>
    </figure>
  </div>
</div>

## The Problem
After installing `Soundcard Oscilloscope` into a Wineprefix. When I tried to run it, it only displays the initial splash screen. If I minimize/maximize the scope application, the language selection dialog box can briefly be seen, but when maximized it's hidden behind the splash screen. It shows up correctly in windows, and by checking the applications settings.ini file after language selection, I found a work around option to use on Linux.

<div class="row">
  <div class="col-md">
    <figure>
       <img src="/images/blog/scope/scope-on-linux.png"
          alt="Image of Scope application stuck on splash screen in linux" class="img-fluid"/>
       <figcaption>First run of Scope application on Linux through Wine</figcaption>
    </figure>
  </div>
  <div class="col-md">
    <figure>
       <img src="/images/blog/scope/scope-on-windows.png"
          alt="Image of scope application working correctly on Windows 10" class="img-fluid"/>
       <figcaption>First run of Scope application on Windows 10</figcaption>
    </figure>
  </div>
</div>

## The Fix
Although the language dialog box is hidden behind the application's splash screen on Linux, the setting can be manually set in an ini settings file. In the Wineprefix
`/drive_c/users/'user-name'/Application Data/scope/settings.ini`
add `language=english`
