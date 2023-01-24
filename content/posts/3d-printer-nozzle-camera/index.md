---
weight: 2
title: "3D Printer Nozzle Camera"
date: 2023-01-23T17:37:16+02:00
lastmod: 2023-01-23T17:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: false
enableEmoji: true

featuredImage: "nozzle-camera-close-up"
resources:
  - name: endoscope-camera-lens
    src: endoscope-camera-lens.png
  - name: endoscope-camera
    src: endoscope-camera.png
  - name: nozzle-camera-close-up
    src: nozzle-camera-close-up.jpg
  - name: nozzle-camera-mount-on-sherpa
    src: nozzle-camera-mount-on-sherpa.png
  - name: nozzle-camera-mount
    src: nozzle-camera-mount.png



tags: ["3d-printer", "Voron", "FDM"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


---

(please ignore the horrible layer lines, this is just a prototype)


<!--more-->

## Concept

A 3d Printer nozzle camera is not a new concept and has been realized by many previously. In fact I have been running a hacked up [nozzle camera on my Ender 3](https://www.printables.com/model/42276-ender-3-borescope-and-induction-probe-mount) for many years now. 

This time around I wanted to build a better looking setup with less wasted space and I wanted this on my [Voron V0.1](https://vorondesign.com/voron0.1) [tri-zero](https://github.com/zruncho3d/tri-zero).

### Why

In addition to it being fun to watch I find it very useful to get the perfect "Squish". I can see in real time the first layer being deposited and adjust it without having to jam my face near a 100°C printer bed.

## Parts

The hardest part of this project is to get a good mini USB camera at a reasonable price. There are many USB endoscope cameras on [Aliexpress for as little as USD 5.-](https://www.aliexpress.com/item/1005004333334502.html?spm=a2g0o.order_list.order_list_main.142.454b1802dQuroK) however they are usually glued into a case and it is difficult to get them out without damaging them. Alternatively you can buy just the camera modules but they are at least 4 times more expensive when it is literally the same module just without a case.

![](endoscope-camera)

In this case I purchased a module in hopes it had the correct focus and wasn't glued.

## Focus

On my Ender 3 have the endoscope 60mm away from the nozzle so it is able to focus on it. This however adds a late of wasted space on the tool head.

Before I purchased the new camera module I asked the seller if the focus was adjustable. Their reply was no but the focus is 2mm to 5m. Of course once I received the module the minium focus distance was clearly 60-70mm and that was even printed on the PCB...

![](endoscope-camera-lens)

On most of these cameras the focus can be adjusted by twisting the tiny ring on the end of the camera. The issue is that this focus ring is usually glued down and depending on the amount of glue you may destroy the whole assembly.

I was luckily able to turn the focus ring under a microscope just enough to get to 20mm focus.


## Mount

![](nozzle-camera-mount-on-sherpa)
[Source](https://github.com/sschueller/sschueller.github.io/raw/master/content/posts/3d-printer-nozzle-camera/sherpa-cowel-v2.step)

To mount the camera I created a little mounting bracket which screws onto my tool head. I have a modded sherpa tool head with extra 3mm inserts allowing me to mount things to my tool like an accelerometer from input shaping or this case the camera.

![](nozzle-camera-mount)
[Source](https://github.com/sschueller/sschueller.github.io/raw/master/content/posts/3d-printer-nozzle-camera/cam-mount-v2.step)

The tool head does hit the front door of the V0.1 if I go all the way to the end of the bed.


## Issues

I glued the camera into position with hot-glue, this works great when printing PLA but I noticed when I print ABS with a bed at 100°C the camera starts to slowly move downwards as the glue is beginning to get soft. I will probably expand on the mount to position the camera so I don't need the glue anymore.

White balance, the camera has an automatic white balance which can sway around when there is a lot of changes in color. A better module would let me fix this.

## Results

Here are some videos the nozzle camera in action

{{< youtube IwfHXu2mzbI >}}


{{< youtube XXwxkiK54yw >}}


https://www.tiktok.com/@3dnozzle

## Upgrades

I need a nozzle wiper, watching a dirty nozzle print is not very satisfying. Sadly [this](https://mods.vorondesign.com/detail/xHsmitgNkpdeQ3tpHImI6A) nozzle wiper doesn't fit a [tri-zero](https://github.com/zruncho3d/tri-zero) so I will need to modify it.

I am looking into getting better image sensors but options are limited.

Alternatively I am considering designing my own PCB with module but I so far have not been able to find a reasonable module at digikey or mouser that I could use. There is also the whole USB/Image Processing that would need to be implemented which seems over kill when I can buy the complete thing for USD 5.-.

Another idea was to use a mirror and not adjust the focus of an existing USB endoscope. This way I could mount it vertically.
