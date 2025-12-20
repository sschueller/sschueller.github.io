---
weight: 2
title: "How to not end up in a Louis Rossmann video"
date: 2025-12-19T18:30:16+02:00
lastmod: 2025-12-19T18:30:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: "How I learned to build a more consumer friendly product without open sourcing everything"
draft: false
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

tags: ["Station Display", "LED", "OpenData", "Right to Repair", "ESP32"]
categories: ["projects", "products"]

toc:
  auto: false
math:
  enable: true


images:
  - '/posts/how-to-not-end-up-in-a-louis-rossmann-video/Display11-small.jpg'


---

What I did to build a more consumer-friendly product without having to open-source everything.

<!--more-->


## Intruduction

A while ago I released my first hardware product, the [Station Display](https://www.stationdisplay.com/). During the process of turning what originally was just a [project into a product](https://sschueller.github.io/posts/turning-a-project-into-a-product/), I needed to make many decisions on how to make it usable enough for the average person and at the same time mass-producible.

After going through this process myself, I somewhat understand why companies are becoming less consumer friendly even when they aren't specifically trying to lock out users. It's just easier and requires less work.

However, I believe it is possible to sell a product that depends on an online service and make it consumer friendly without having to bend over backwards and give up all your IP.

Here a few points and decisions I made in addition to what is required by law to make my product more consumer friendly. 

## Regulatory requirements (bare minimum)

In Switzerland there are several minimum legal requirements similar to those in the EU. Everyone needs to follow these, but that doesn't make your product consumer friendly.

A CE mark or Swiss equivalent (CH) is required. Its purpose is to protect customers from dangerous chemicals and devices causing interference, etc.

Another one is a minimum two-year warranty. For me, this means I need to keep enough parts and whole units on hand to replace any customer's unit if an issue occurs. For example, I need to keep stock of each LED panel batch, as the colors vary slightly; if I need to replace a panel, I would need to replace all three. For a small vendor like me, this is not insignificant, especially since my LED supplier only gives me a one-year warranty.

Additionally, I need to have insurance for 10 years after the last sale in case there are any issues with the device and I need to take old units back for recycling (VREG).

In Switzerland it is also common to offer a 14-day return policy for online purchases, although not required by law. Some trade associations will require you to do this if you want to be a member.

## Making a difference


### Software

The [Station Display](https://www.stationdisplay.com/) consists of three pieces of software: the firmware on the device, the configuration app ([Android](https://play.google.com/store/apps/details?id=com.stationdisplay.configurator) and [iOS](https://apps.apple.com/us/app/station-display/id6462993011)), and the server providing the data.

Although this makes the setup and use of the device very easy, the device would stop working if I closed shop, leaving people with a large paperweight.

##### Configuration App

When I initially designed the configuration app, I built it in such a way that it does not require registration nor does it need any type of activation. If you have a [Station Display](https://www.stationdisplay.com/), you can connect to it and configure it. The app does use my server to offer a search for the station you want to display; however, you can also just enter its ID, which you can find in the official government [GTFS feed](https://data.opentransportdata.swiss/de/dataset/timetable-2026-gtfs2020).

I also published the [Bluetooth characteristics](https://github.com/TechdroidInc/stationdisplay/blob/main/BLE.md) for configuration, and the device can be configured with any generic Bluetooth tool like [nRF Connect](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp). So even if my app gets removed from either app store, the device can still be configured. This also benefits people who don't have an Apple or Android device.

Since I publish the [characteristics](https://github.com/TechdroidInc/stationdisplay/blob/main/BLE.md) and do not lock down the Bluetooth, I have it shut off a few minutes after boot. Data is received via WiFi, so Bluetooth is not needed after initial configuration, and this satisfies CE IoT security requirements.

The config app is not open source, although there isn't really any IP. All it does is connect to the [Station Display](https://www.stationdisplay.com/) using the published [Bluetooth characteristics](https://github.com/TechdroidInc/stationdisplay/blob/main/BLE.md) and offer a UI.

I may release the app source, but it requires a bit of time for me to prepare it for this, which I would rather invest in adding new features to the [Station Display](https://www.stationdisplay.com/).

##### Device Firmware

I purposely do not lock the firmware or trigger the e-fuse, so you can flash another firmware onto the device ([ESP32S3](https://www.espressif.com/en/products/socs/esp32-s3)). OTA flashing, however, is only possible via my OTA server (unless other firmware is flashed via USB that supports other OTA methods), also because of CE requirements. The original firmware can be [downloaded](https://github.com/TechdroidInc/stationdisplay/blob/main/FIRMWARE.md) and re-flashed, restoring all functionality.

Ideally, I would want customers to be required to confirm something before being able to flash over the firmware, because it is possible to damage the hardware with bad firmware, but I don't have that capability at this time. I also don't expect a large number of customers to flash firmware, damage the device, and file a warranty claim.

The current firmware is not open source, but it will be released when I stop selling the device. There is an [older version available open source](https://github.com/sschueller/vbz-fahrgastinformation) which I made for the original [project](https://sschueller.github.io/posts/vbz-fahrgastinformation/).

##### Server Software

The server software collects the freely available government data from [opentransportdata.swiss](https://opentransportdata.swiss/) and processes it for the [Station Display](https://www.stationdisplay.com/). It mixes the live data with the scheduled data and provides a consistent data set for the displays. The [Station Display](https://www.stationdisplay.com/) does not have enough processing power to process the raw GTFS feeds from [opentransportdata.swiss](https://opentransportdata.swiss/), and the feed also changes from time to time, which would require firmware updates.

![GTFS-RT Import Totals](gtfs-rt-stats)

OTA updates are also provided by the server to the [Station Display](https://www.stationdisplay.com/).

The server software is not open source, and the service is only available to purchased devices at this time. However, the protocol is [documented](https://github.com/TechdroidInc/stationdisplay/blob/main/API.md), and you can switch to your own server in the existing firmware. This allows you to have all the features with your own data. The data I use is open, and one can build a replacement or alternative server using the data feed from [opentransportdata.swiss](https://opentransportdata.swiss/). Alternatively, you could also feed the [Station Display](https://www.stationdisplay.com/) a schedule for your model train set.

### Hardware 

I published the [schematic and BOM](https://github.com/TechdroidInc/stationdisplay/tree/main/PCB/esp32-s3-rev-k). Although this allows people to easily replicate my device, I don't see there being anything special about the PCB design that isn't easily replicable anyway.

![PCB](pcb)

## Conclusion 

I find the regulatory minimum requirements much more challenging and costly than anything else I did to make my product more consumer friendly, yet they have a big impact in the long term. It just requires a bit of thinking about how a customer would want to use the device and what the customer would do when you no longer offer the service.

##### Could I do more? 

Yes, I could release all the software I wrote as open source now, as well as all the manufacturing files for the frame, glass, and CAD models.

##### Why don't I now? 

Since I want to sell a few units, I think this is the most balanced approach.

If I stop selling the device, I will release the rest as open source.

