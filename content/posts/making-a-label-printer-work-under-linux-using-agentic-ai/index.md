---
weight: 2
title: "Making a label printer work under Linux using agentic AI"
date: 2026-01-15T18:30:16+02:00
lastmod: 2026-01-15T18:30:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: true
enableEmoji: true

featuredImage: "station-display-1"
resources:
  - name: gtfs-rt-stats
    src: 2025-12-19-215231_708x301_scrot.png
  - name: station-display
    src: Display10-small.jpg
  - name: station-display-1
    src: Display11-small.jpg
  - name: pcb
    src: pcb-kicad.png

tags: ["Agentic AI", "AI", "Kilocode", "Label Printer"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


images:
  - '/posts/how-to-not-end-up-in-a-louis-rossmann-video/Display11-small.jpg'


---
Making a label printer work under Linux using agentic AI

<!--more-->

## Intruduction

https://de.aliexpress.com/item/1005009489657919.html


https://de.aliexpress.com/item/1005002017283862.html

A while a go I purchased this cheap Chinese label printer. Sadly when I set it up under Linux although somewhat  supported the print results were bad while the same printer produced decent prints in windows or via the android app. I could not get better prints with cups no matter what I tried. I also tried to set some default paper sizes but it was a huge pain .

Until recently I would send a pdf of the labels I wanted to print to my phone and use the app to print via Bluetooth. Alternatively I would use a windows VM to print via USB.

I thought there has to be a better way. So here is what I did.

## Decompiling the android app

The android app works very well via Bluetooth and needs no setup. Maybe I can get better prints via Bluetooth than USB under Linux. I could then also place my printer further away from my PC.

So I extracted the APK from my phone using Apk Extractor. https://play.google.com/store/apps/details?id=braveheart.apps.apkextract&hl=en

I then decompiled it using [Jadx](http://www.javadecompilers.com/apk) online and download those files. 

Initially my plan was to go through the code and figure out what Bluetooth characteristics I need to use and what to send. This can get tedious fast and take a lot of time.

However I was feeling lazy and did not want to spend my day-off deciphering decompiled code.

## Kilocode to the rescue 

I decided to give it a try using kilocode. Maybe an agentic ai has an easier time to decipher a decompiled APK then I do and I recently setup a lot of new models in litellm to work with my kilocode setup.

I placed the extracted APK in a new project and told the agent that I wanted to to use the code to generate a go version. Why go? I have had a lot of good results with go vs python when using deepseek. 

Initially I tried it using deepseek coder which usually provides good results for dirt cheap but I wasn't getting anywhere. Although it would connect to the printer it was not working. Something was off but I did not know what. Most likely some initialization that was missing.

Before trying to debug it myself I decided to first switch to Gemini 3 pro and although initially it also did not work I was able to work with it until the printer responed. 

Sadly Gemini is slow and a lot of the time I get overload errors. 

Once I had a working go app I asked the agent to make a new folder and build a web only version for chrome (not all browsers Support Bluetooth). This worked in the first iteration.

I now have a web UI where I can dump a pdf, convert it and select from options the printer offers. I can also print a test pattern to alight the print which the regular app or driver can't do.

If you have a similar printer I would be curious to know if it also works with yours.

You can download what the AI produced here (minus the decompiled APK, for reasons...). I haven't reviewed the code so there may be some oddities and dead ends...





