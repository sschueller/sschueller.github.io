---
weight: 2
title: "Mesh client build log - part 2"
date: 2025-09-03T18:30:16+02:00
lastmod: 2025-09-03T18:30:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: "A build log of a Meshtastic/Meshcore client device"
draft: true
enableEmoji: true

featuredImage: "mesh-1"
resources:
  - name: keyboard-pcb-4
    src: 2025-08-31-135534_793x744_scrot.png
  - name: mesh-1-p1
    src: f0xrur4ryuff1.png


tags: ["Meshtastic", "Meshcore", "LORA"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true


images:
  - '/posts/meshtastic-client-update-1/P_20250829_200306.jpg'


---

The Main board and module PCBs

<!--more-->

## Main Board PCB

The main board has several section. The most important is the power and battery management. The rest are just connections to the LCD, Keyboard PCB and the module PCB.

### Battery charging



### Battery safety

This would have been way easier if I had used one 18650, a  pack or a fixed size sell. Making the batteries removable and in a parallel configuration makes this a whole lot more complicated.

I need to account for the situation if the batteries are inserted in reverse and if there is a short across one of the holders resulting in the other battery being shorted.

Additionly there is a risk of high current flow between the two batteries if they are not equaly charged.

I looked at a few other projects like the uConsole which uses two fuses and two diodes to prevent back flow I did not like this as the diodes would eat quite a bit of voltage and there is no protection for batteries balancing at high current.

The best solution I could come up with is placing two 

### Power management



## CPU / Radio PCB

The main "CPU" I decided to go with is the [Heltec Wireless Shell V3](https://heltec.org/project/wireless-shell-v3/), which is an ESP32S3 and SX1262 in one module.

The module will sit on a PCB that fits in an M.2 Type G slot. Type G is "Reserved for custom use" and does not fit in a "regular" M.2 slot like mSata devices etc.

![Radio Module PCB](module-pcb-1)

![Radio Module PCB](module-pcb-2)

At this time I have added a test connector for the GPS PCB. If I can get the GPS functionality to work I may add it directly to this PCB. The pinout of the connector is the same as the ATGM336H module.


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