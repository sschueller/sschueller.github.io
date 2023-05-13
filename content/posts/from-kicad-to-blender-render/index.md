---
weight: 2
title: "Generating fancy 3d renders from KiCad exported PCBs"
date: 2023-05-13T17:00:16+02:00
lastmod: 2023-05-13T17:00:16+02:00
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

Now export the 3d model of the entire PCB with the following options set:

![KiCad 3D export](kicad-3d-export)

## Blender Import




## Eye-Candy

This is an export from KiCad into Blender which textures applied as well as animated. 

{{< youtube svU7NyfJJKY >}}