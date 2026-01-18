---
weight: 2
title: "Making a label printer work under Linux using agentic AI"
date: 2026-01-18T18:30:16+02:00
lastmod: 2026-01-18T18:30:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: false
enableEmoji: true

featuredImage: "mini-labels"
resources:
  - name: label-printer-UI
    src: 2026-01-18-185249_668x826_scrot.png
  - name: mini-labels
    src: 531647410-a39b1aa5-5e17-4656-906e-e92134ea1e62.jpg
  - name: decompiled
    src: 2026-01-16-221935_202x277_scrot.png
  - name: kilo-code
    src: 2026-01-18-191234_831x554_scrot.png


tags: ["Agentic AI", "AI", "Kilocode", "Label Printer"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


images:
  - '/posts/making-a-label-printer-work-under-linux-using-agentic-ai/531647410-a39b1aa5-5e17-4656-906e-e92134ea1e62.jpg'


---

<!--more-->

## Intruduction



A while ago I purchased this "cheap" [Chinese label printer](https://de.aliexpress.com/item/1005009489657919.html) which can print different sized labels like [these](https://de.aliexpress.com/item/1005002017283862.html). Sadly, when I set it up under Linux, although somewhat supported, the print results were bad, while the same printer produced decent prints in Windows or via the [Android app](https://play.google.com/store/apps/details?id=com.print.label). I could not get better prints with CUPS no matter what I tried. I also tried to set some default paper sizes, but it was a huge pain.

Until recently I would send a PDF of the labels I wanted to print to my phone and use the app to print via Bluetooth. Alternatively, I would use a Windows VM to print via USB.

I thought there had to be a better way. So here is what I did.

## Decompiling the android app

The [android app](https://play.google.com/store/apps/details?id=com.print.label) works very well via Bluetooth and requires no setup. Perhaps I can get better prints via Bluetooth than via USB under Linux. I could then also place my printer further away from my PC.

So I extracted the APK from my phone using APK Extractor. https://play.google.com/store/apps/details?id=braveheart.apps.apkextract&hl=en

I then decompiled it using [Jadx](http://www.javadecompilers.com/apk) online and download those files. 

Initially, my plan was to go through the code and figure out what Bluetooth characteristics I needed to use and what to send. This can get tedious fast and take a lot of time.

However I was feeling lazy and did not want to spend my day off deciphering decompiled code.

## Kilocode to the rescue 

I decided to give it a try using [Kilocode](https://kilo.ai/). Maybe an agentic AI has an easier time deciphering a decompiled APK than I do, and I recently set up a lot of new models in [LiteLLM](https://www.litellm.ai/) to work with my Kilocode setup.

### Go Version

I placed the extracted APK in a new project and told the agent that I wanted to to use the code to generate a go version. Why go? I have had a lot of good results with go vs python when using deepseek. 

```text {open=true, lineNos=false, wrap=true, header=true}
Using the code in com.print.label_v66/sources/com/print/label/bluetooth and the other folders. Write me a go script that can print a PDF via bluetooth on a Linux PC in the "desktop" folder. I also need to be able to set the paper size for the label to be printed. The printer's bluetooth ID is DD:0D:30:02:63:42
```

Initially, I tried using DeepSeek Coder, which usually provides good results for dirt cheap, but I wasn't getting anywhere. Although it would connect to the printer, it was not working. Something was off, but I did not know what. Most likely, some initialization was missing.

Before trying to debug it myself, I decided to first switch to Gemini 3 Pro. Although initially it also did not work, I was able to work with it until the printer responded.

Sadly, Gemini is slow and a lot of the time I get overload errors but eventually I got a go app that worked and I could call from the command line.

```bash
go run print_label.go \
--bluetooth \
--printer-id DD:0D:30:02:63:42 \
--tspl \
--paper-size 100 \
--paper-height 150 \
--pdf Labels-Sample.pdf \
--margin-x 5
```

After a few more back-and-forths with the agent, I had all the features I wanted working.

### Web Version

Once I had a working Go app, I asked the agent to make a new folder and build a web-only version for Chrome (not all browsers support Bluetooth). This worked in the first iteration.

![Kilo code response](kilo-code)

I now have a web UI where I can upload a PDF, convert it, and select from the options the printer offers. I can also print a test pattern to align the print, which the regular app or driver cannot do.

![Label Printer Web UI](label-printer-UI)

If you have a similar printer, I would be curious to know if it also works with yours.

You can download what the AI produced [here](https://github.com/sschueller/xp-d463b-pdf-printer) (minus the decompiled APK, for reasons...). I haven't reviewed the code, so there may be some oddities and dead ends...





