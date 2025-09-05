---
weight: 2
title: "Mesh client build log - part 1"
date: 2025-08-29T18:30:16+02:00
lastmod: 2025-08-29T18:30:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: "A build log of a Meshtastic/Meshcore client device"
draft: false
enableEmoji: true

featuredImage: "mesh-1"
resources:
  - name: keyboard-pcb-4
    src: 2025-08-31-135534_793x744_scrot.png
  - name: mesh-1-p1
    src: f0xrur4ryuff1.png
  - name: keyboard-pcb-1
    src: 2025-08-31-135534_793x744_scrot.png
  - name: keyboard-pcb-2
    src: 2025-08-31-135559_624x383_scrot.png
  - name: keyboard-pcb-3
    src: 2025-08-31-135548_984x893_scrot.png
  - name: proto-2-front
    src: 2025-08-31-135732_860x702_scrot.png
  - name: main-pcb-1
    src: 2025-08-31-135813_803x747_scrot.png
  - name: proto-2
    src: 2025-08-31-135841_994x670_scrot.png

  - name: module-pcb-1
    src: 2025-08-31-141019_1339x499_scrot.png
  - name: module-pcb-2
    src: 2025-08-31-141039_1183x586_scrot.png


  - name: mesh-1
    src: P_20250829_200306.jpg
  - name: mesh-1-top
    src: P_20250829_200322.jpg
  - name: mesh-inside
    src: P_20250829_200351.jpg
  - name: mesh-leyboard-pcb-photo-1
    src: P_20250831_135633.jpg
  - name: mesh-leyboard-pcb-photo-2
    src: P_20250831_135650.jpg

  - name: Rii-518BT
    src: 2025-08-31-141417_924x579_scrot.png


tags: ["Meshtastic", "Meshcore", "LORA"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


images:
  - '/posts/meshtastic-client-update-1/P_20250829_200306.jpg'


---

My goal with this project is to build a fully open-source [Meshtastic](https://meshtastic.org/)/[Meshcore](https://meshcore.co.uk/) compatible client that is more "rugged" than what is currently available on the market. If there is enough interest, I will consider manufacturing a few and getting CE etc. certification done. If you are interested, please join the mailing list below.

<!--more-->

## Features / Wants

This is a list of things I want it to be able to do, but they might not all make it:

- Rugged case (prototype out of Aluminum, production would be Nylon filled ABS?)
- Some water resistance (rain)
- Modular Radio (Initially a ESP32S3 with SX1262)
- Good keyboard
- Long battery life (at least 2 x 18650s) and battery charger
- Carry strap
- Good display (IPS, 2.8")
- GPS
- NATO Rail


## Where to start

### Case Design

I'd like to start in Fusion 360 and design a case. Here is my first version:

![Initial drawing](mesh-1-p1)

After 3D printing several versions, checking LCD dimensions and where PCBs could go, I ended up with the following more or less final design:

![Refined drawing](proto-2)

#### Hooks

The hooks on the top can be moved to any other corner, so you can change the way the device hangs from a lanyard.


#### Sides

The NATO rails on the side are more decorative than anything useful. They can be removed and I am thinking of instead adding rubber bevels to make the device more comfortable in the hand.


#### Ports and buttons

![Top](mesh-1-top)

I am only exposing the USB port for recharging and programming. The other items on the top are the Power and possibly a reset button as well as the antennas

#### Antennas

There are two SMA connectors on the top, one for LoRa and one for WiFi/BLE.



### PCBs

There are 4 PCBs in this project. I may possibly reduce it to 3 but I am not sure yet.

The core idea is to have a mainboard which has the LCD and keyboard PCB connected to it. It would also have the batteries, charger and USB port. The Main CPU (ESP32S3) and LoRa radio would be on a custom M.2 Type-G type board that interfaces with the mainboard. This allows switching of the radio and later CPU upgrades.

The GPS circuit is currently also on a separate PCB but it may be possible to place those components directly on the M.2 board.

The keyboard is also a separate self-contained PCB with ESP32S3 on it.

#### Main Board

![Main PCB v1](main-pcb-1)



#### CPU / Radio

The main "CPU" I decided to go with is the [Heltec Wireless Shell V3](https://heltec.org/project/wireless-shell-v3/), which is an ESP32S3 and SX1262 in one module.

The module will sit on a PCB that fits in an M.2 Type G slot. Type G is "Reserved for custom use" and does not fit in a "regular" M.2 slot like mSata devices etc.

![Radio Module PCB](module-pcb-1)

![Radio Module PCB](module-pcb-2)

At this time I have added a test connector for the GPS PCB. If I can get the GPS functionality to work I may add it directly to this PCB. The pinout of the connector is the same as the ATGM336H module.

#### Keyboard

This is the first of the 4 PCBs I am finishing. As of right now, I have decided to have the keyboard be an independent device which communicates with the main PCB over I2C. To deal with phantom presses etc., I decided to just go with a full ESP32S3 so I have enough GPIOs as the companion chips I found for keyboards either are STM based or add too much overhead. The ESP32 radios will be disabled and the antenna pin is not connected.

The square cutout is for the GPS antenna, so it can be placed outside the main case but under the keyboard membrane.

There is an FPC connector to the mainboard and a JST header for programming.


![Keyboard PCB](keyboard-pcb-4)


![Keyboard PCB](keyboard-pcb-3)


![Keyboard PCB](keyboard-pcb-2)



![Keyboard PCB](mesh-leyboard-pcb-photo-2)


![Assembled Keyboard PCB](mesh-leyboard-pcb-photo-1)


#### GPS

Initially, I will try to get it to work with an ATGM336H module. I may at a later time integrate this directly on the CPU PCB.

### The Keyboard

For the keyboard, I want "clicky" buttons and not just some soft rubber. I based my design off the Rii 518BT keyboard which is a very nice small keyboard with a good feel.

![Rii 518BT](Rii-518BT)

I have not yet figured out how I am going to get the "keys" manufactured. I will be contacting vendors when I get to this point to check costs etc.
I also added backlighting to the PCB for the keyboard.


## Next Up

Next on my list of things to do:

- Program and test keyboard PCB
- Finish design of main PCB and the module PCB
- Test all PCBs together including battery charging and LCD

Later:
- Figure out gasket situation and keeping water out
- Finalize case design and have an aluminum version made
- Create a fork of Meshcore and Meshtastic for the device

## Mailing list

If you are interested, please sign up for the mailing list. Also, please let me know if you have ideas or suggestions.

[Click here](https://survey.tei.li/s/cmezpb9yu000a13xazar0wu0t)