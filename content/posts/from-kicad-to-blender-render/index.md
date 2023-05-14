---
weight: 2
title: "Generating fancy 3d renders from KiCad exported PCBs"
date: 2024-05-13T17:00:16+02:00
lastmod: 2024-05-13T17:00:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: "Generating 3d PCB renders"
draft: false
enableEmoji: true


featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png
  - name: kicad-3dmodel-edit
    src: kicad-3dmodel-edit.png
  - name: kicad-3dmodel-gab
    src: kicad-3dmodel-gab.png
  - name: kicad-3dmodel
    src: kicad-3dmodel.png
  - name: kicad-3d-export
    src: kicad-3d-export.png
  - name: blender-jagged-barell
    src: blender-jagged-barell.png
  - name: blender-smooth-barell
    src: blender-smooth-barell.png
  - name: blender-fixed-smoothness
    src: blender-fixed-smoothness.png
  - name: blender-auto-smooth
    src: blender-auto-smooth.png




tags: ["PCB", "KiCod", "EE", "Blender", "3d"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


---
(Blender Render of KiCad Exported PCB)


<!--more-->


## Introduction

This is a step by step of how I export my PCBs from KiCad and then import them into Blender in order to produce fancy looking renders. Most of it is based on this [fantastic 4.5 hour video](https://www.youtube.com/watch?v=1Pjr0xkuyhU) by {{< person url="https://twitter.com/diconx" name="Sebastian Staacks" picture="https://pbs.twimg.com/profile_images/1439617218585763844/L4gtnhTs_400x400.jpg" >}} however a few things have become simpler due to KiCad doing a better job at exporting. If you get stuck I recommend watching the video, I have summarized what I use but there is a lot of good information in the video especially if you aren't very familiar with Blender.

## Preparation in KiCad

In order to get good results and make your life easier I recommend you make a few changes in KiCad before you export the 3d file.

### 3d Models

Add 3d models to all your parts. Most common built-in KiCad parts (resistors, capacitors etc.) have models but you will need to add models for the parts that don't. 

Easiest is to just import a step or wrl file of the part in the "3D Models" tab in the footprint properties (PCB View). If you have made your own footprint I recommend you define this under the properties of the custom footprint then it will be available everywhere you used this part.

![KiCad Model Edit](kicad-3dmodel-edit)

{{< admonition info "Note" true >}}
Important here is to position the part correctly. Make sure it doesn't not touch the PCB or any part of a hole if applicable. There should be a bit of a cap so that we can then split the part off in Blender. I have not had to edit any built-in KiCad parts as the gab was big enough.
{{< /admonition >}}

![KiCad Model Gap](kicad-3dmodel-gab)

### Export
Check in the 3d view that everything is the way you want.
![KiCad 3D view][kicad-3dmodel]

Now export the 3d model of the entire PCB as a ```wrl``` with the following options set:

![KiCad 3D export](kicad-3d-export)

## Blender 

### Import

Create a new project and remove the cube. Then import the exported ```wrl```.

Resize the imported PCB (Press ```s```) so that the existing light source and camera seem about the right size. If the PCB needs rotating press ```r``` and the axis for example ```x```, then the amount in degrees such as ```90```.

Also reposition the PCB onto the origin using ```g``` and on which axis ```z``` etc.

### Separate the Parts from the PCB

First we need to join everything into one piece. Do this in ```Object Mode```, select the PCB and all its parts using ```b``` with the mouse or if that doesn't work a combination of ```w``` and using the mouse. 
{{< admonition warning "Note" true >}}
Do not select the light source and the camera.
{{< /admonition >}}

Now press ```ctrl + j``` to join all the parts together

Switch to ```Edit mode``` by pressing ```tab``` and press ```m```. Select ```By Distance```. Press ```p``` select ```By Loose Parts```. 

All the parts should be separated now. You can check this by switching back to ```Object Mode``` with ```tab``` and clicking the the parts.

{{< admonition info "Note" true >}}
If you have parts that are in 2 pieces you can shift select the pieces and join them with ```ctrl + j```. 

If you have parts stuck to the PCB or stuck together. In ```Edit mode``` select a vertices of the piece to separate and press ```ctrl + L```. Now use shift select to unselect the parts that don't belong together and then use ```p``` to separate them.
{{< /admonition >}}

### Smooth Parts

Some parts may not have smooth curves. The correct this we can use the ```Shade Smooth``` feature. In ```Object Mode``` select the part to smooth, right click on it and select ```Shade Smooth```. Initially the part may look too smooth as it is using the global value to smooth out the part. Click the ```Object Data Properties``` tab on the right side and under ```Normals``` adjust the ```Auto Smooth``` value until the part looks good.

No Smoothing:
![Blender Jagged part](blender-jagged-barell)

Too Much Smoothing:
![Blender Smoothed part](blender-smooth-barell)


![Blender Smooth adjustment](blender-auto-smooth)


Just Right:
![Blender Smoothed parts](blender-fixed-smoothness)

Do this for all parts that need it

### Name Parts

For easier management I like to name all my parts in the Scene. 


### Shading

TODO

### Background Lighting

For a nice render you will need some more advanced lighting than the standard point source lights. You can do this with backgrounds.


TODO

### Animating



### Rendering



## Eye-Candy

This is an export from KiCad into Blender which textures applied as well as animated. 

{{< youtube svU7NyfJJKY >}}