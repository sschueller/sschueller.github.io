---
weight: 2
title: "Wiring my home with Fiber"
date: 2022-10-29T16:37:16+02:00
lastmod: 2022-10-29T16:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: false
enableEmoji: true

featuredImage: "ftth"
resources:
  - name: ftth
    src: ftth.jpg
  - name: network-diagram
    src: /images/network-diag.png

tags: ["Fiber", "FTTH", "Network"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true

images:
  - /images/network-diag.png
---

<!--more-->

> Many thanks to [Michale Stapelberg](https://michael.stapelberg.ch/) for the inspiration and information about his setup. Also thanks to [Init7](https://www.init7.net/de/init7-empfehlen/) (for the great service), [r/FiberOptics](https://old.reddit.com/r/FiberOptics/) and [FS](https://www.fs.com) for providing me with what I needed to get this setup going.

{{< admonition about "Init7 Discount" true >}}
If you find this article useful and you are considering [Init7](https://www.init7.net/en/init7-empfehlen/) as your provider you can use my referral code "20700408098" to get CHF 111.- off hardware and also support me paying off the splicer :smile:.
{{< /admonition >}}

{{< admonition warning "Warning" true >}}
I am by no means an expert when it comes to fiber. This page describes what I did but it may all be wrong and not the right way to do this.
{{< /admonition >}}

## Planning

Alway good to have a plan before you start shopping.

My network diagram

![Network Diagram](/images/network-diag.png)

I came to the following requirement for my setup:

- At least 10gbits to each PC
- At least one fiber link per floor
- Ability to utilize 25gbit internet connection
- ~~no splicing Fiber~~
- Outlets, no direct wiring
- ~~Keep the costs down (reasonable)~~
- No noisy Switches or Routers
- No drilling, no cutting holes into the house

In the end some of these didn't pan out as I will explain further down.

### Type of fiber

{{< admonition info "TLDR" true >}}
I went with singlemode fiber with LC connectors in a G.657.A1 jacket. Connectors polished with APC for wall outlet and UPC for the SFP module side.
{{< /admonition >}}

{{< admonition danger "Good to know" true >}}

- APC and UPC polished fibers do not mate, don't connect the two together, it will not work. Always connect APC to APC and UPC to UPC
- You can not mix multimode with singlemode. They are completely different. Pick one and stick with it.
- Do not bend fiber beyond the rated bending radius.
  {{< /admonition >}}

#### Singlemode vs multimode

From that I can gather most internal wiring of fiber use multimode but there is no "rule" against using singlemode.

Since the cost difference is not that big now I decided to go with singlemode. It allows for upgrading to higher bandwidth by just replacing the SFP modules on each end. My ISP plans on proving 100gbits in about 2 years so I don't need pull new fiber if I decide to upgrade.

#### Connectors

There are a lot of connectors for fiber but generally in FTTH I only see LC connectors. The benefit is that they are quite small and can therefore be pulled through conduits without needing to terminate the fiber at a later time (if the conduit permits it).

![Fiber UPC LC Connector](/images/lc-connector-fs.jpg)

#### Polish

The most common polishes appear to be APC (Green) and UPC (Blue). The incoming FTTH line from the street is APC (Green) most SFP modules are UPC (Blue). As I am not directly connecting devices I will be using APC (Green) to APC (Green) inside walls and APC (Green) to UPC (Blue) wall to device. So to avoid any confusion about polish. All outlets are APC (Green), all devices are UPC (Blue).

![Fiber UPC Polish](/images/apc-vs-upc-fs.png)

#### Splicing

![Signal Fire Splicer](/images/Signalfire-AI-9.jpg)

My plan was to buy pre-terminated fibers however it turned out that the conduits are too small to pull pre-terminated fiber through it. For this reason I decided to purchase a [fiber fusion splicer](https://www.aliexpress.com/item/33012836265.html) for around CHF 600 which blew my budget.

#### Fiber stock

![Fiber Spool](/images/P_20221004_142707.jpg)

For material I chose a G.657.A1 4 core fiber with a 2.2mm LSFH jacket. The minimum turning radius for G.657.A1 is 10mm which should also be enough.

![Fiber Radius](/images/radius.jpg)

#### Pig-tails

![Pig-Tails](/images/12-colors-LC-APC-Fiber-Pigtail-with-Single-mode-G652D-G657A1-G657A2.jpg)

In order to connect ends to the fiber using a fusion splicer I used pig-tails. These are one side pre-terminated pieces of fiber. [12 colors LC/APC Fiber Pigtail with Single mode](https://www.aliexpress.com/item/32809109613.html) I went with G.657.A2 which have a min bending radius of 7.5mm.

### SFP modules

#### SFP, SFP+, SFP28, etc.

First thing was to decide what speed I wanted to use. I settled on 10gbits for now as I can do this with someone lower cost SFP+ modules and switches. SFP28 is still quite costly especially switching hardware.

The router requires a SFP28 module to connect to the ISPs 25gibt line which was provided by the ISP. It is running in a [Broadcom BCM957414A4142CC Cloud](https://www.digitec.ch/de/s1/product/broadcom-assy-top-bcm957414a4142cc-cloud-ethernet-netzwerkkarte-15730588) PCIe card.

#### BiDi

I also decided to go withe BiDi modules which can do RX and TX on the same fiber on two different wavelengths. To achieve this however you need opposite modules on each end so you need to plan a bit. The ISP also uses BiDi modules for their FTTH.

![SFP+ BiDi module](/images/fs-sfp-sm-bidi.jpg)

#### Fiber attenuators

When you use singlemode SFP modules you may need an attenuator if the fiber run is short. However in this case [FS](https://www.fs.com) confirmed that as long as I used their same matching modules I would not need any.

#### Module coding

Coding is what will ruin your day. Thanks to companies like Cisco and Dell we have to deal with such crap when SFP is a standard yet they code them specifically for their devices. Luckily companies like [FS](https://www.fs.com) will do free coding for what ever you need when you purchase a SFP module or you can even purchase their coding box which will let you change the coding at a later date. Other companies will even send you a free box but most of these boxes can only change the coding of the modules from those companies.

Some devices don't really care what coded module is inserted but I didn't want to take that risk and decided to plan which module goes into which device.

#### Module locations, TX/RX and coding

| Device   | SFP+/SFP28 Module                | Dir. (Internet) | Coded    |
| -------- | -------------------------------- | --------------- | -------- |
| Router   | SFP28 (init7)                    | In              | Broadcom |
| Router   | SFP-10G-BX (1330nm-TX/1270nm-RX) | Out             | Intel    |
| Switch 1 | SFP-10G-BX (1270nm-TX/1330nm-RX) | In              | Microtik |
| Switch 1 | SFP-10G-BX (1330nm-TX/1270nm-RX) | Out             | Microtik |
| Switch 1 | SFP-10G-BX (1330nm-TX/1270nm-RX) | Out             | Microtik |
| Switch 2 | SFP-10G-BX (1270nm-TX/1330nm-RX) | In              | Microtik |
| Switch 2 | SFP-10G-BX (1330nm-TX/1270nm-RX) | Out             | Microtik |
| Switch 3 | SFP-10G-BX (1270nm-TX/1330nm-RX) | In              | Microtik |
| Switch 3 | SFP-10G-BX (1330nm-TX/1270nm-RX) | Out             | Microtik |
| PC 1     | SFP-10G-BX (1270nm-TX/1330nm-RX) | In              | Intel    |
| PC 2     | SFP-10G-BX (1270nm-TX/1330nm-RX) | In              | Intel    |

### SFP modules PCIe cards

In order to use the SFP+ modules you need the correct PCIe cards on your PC. I was able to find some used Intel SFP+ cards which work very well with Linux and many other OSs.

![Intel SFP+ PCIe](/images/intel-sfp-pcie.jpg)

### SFP switches

Generally SFP+ Switches are not cheap and quite loud but thanks to Mikrotik there is a fan-less switch with 4 ports which is more than enough for me. I am using one of these per floor which covers my needs. It also has one RJ45 copper gigabit port so I can split of into another switch for devices that don't need 10gbit (Printers..).

![Mikrotik CRS305](/images/mikrotik-crs305-1g-4s-in-gigabit-switch.png)

### Outlets

I did not want to run fiber directly from device to device but instead install fiber outlets in the rooms. There are different options for this but I opted for the ["FTTH Squeeze OTO"](https://shop.zidatech.ch/de/shop/fiber-in-the-home-fith/squeeze-oto-dose/ftth-flach-ap-anschlussdose-ohne-pigtails-nur-kupplungen.html) which is a outlet that can be installed behind and existing one without having to disconnect the existing outlet. So for example I can run fiber in the same conduit as power or cable TV and the cover plate is just moved out a bit. This design is specifically for Swiss/European style outlets. These are only available with APC (Green) polish couplers.

{{< youtube ARSpp4B9-X4 >}}

I also used these [Delock Keystone Anschlussdose, 2 Port](https://ch.elv.com/delock-keystone-anschlussdose-2-port-113270) with [APC Keystone fiber couplers](https://www.aliexpress.com/item/1005001493778225.html) outlets.

### Router

Hardware SFP28 routers are expensive and very loud. Since this is going into a home I decided to custom build a router using a simple PC running [Proxmox](https://www.proxmox.com/) and [Opnsense](https://opnsense.org/). Although the cost ist sill not low I can use this system for other things as well such as a [Pi-hole](https://pi-hole.net/).

| Part                                                                                                                                                          | Cost            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| [AsRock B550 Taichi (AM4, AMD B550, ATX)](https://www.digitec.ch/de/s1/product/asrock-b550-taichi-am4-amd-b550-atx-mainboard-13348335)                        | CHF 278.-       |
| [Corsair 4000D Airflow (ATX, mATX, Mini ITX, E-ATX)](https://www.digitec.ch/de/s1/product/corsair-4000d-airflow-atx-matx-mini-itx-e-atx-pc-gehaeuse-13552873) | CHF 112.-       |
| [Noctua NH-L12S (7 cm)](https://www.digitec.ch/de/s1/product/noctua-nh-l12s-7-cm-cpu-kuehler-6817433)                                                         | CHF 61.90       |
| [AMD Ryzen 7 5800X (AM4, 3.80 GHz, 8 -Core)](https://www.digitec.ch/de/s1/product/amd-ryzen-7-5800x-am4-380-ghz-8-core-prozessor-13987918?supplier=406802)    | CHF 336.-       |
| [Kingston A400 (480 GB, M.2 2280)](https://www.digitec.ch/de/s1/product/kingston-a400-480-gb-m2-2280-ssd-12518072)                                            | CHF 57.30       |
| [G.Skill Aegis (2 x 8GB, DDR4-2666, DIMM 288 pin)](https://www.digitec.ch/de/s1/product/gskill-aegis-2-x-8gb-ddr4-2666-dimm-288-pin-ram-11056536)             | CHF 53.10       |
| [EVGA B5 (650 W)](https://www.digitec.ch/de/s1/product/evga-b5-650-w-pc-netzteil-13751314)                                                                    | CHF 67.20       |
| Matrox Millennium G550 (Single PCIe Lane GFX card)                                                                                                            | CHF 30.- (Used) |
| [Broadcom Assy Top BCM957414A4142CC Cloud](https://www.digitec.ch/de/s1/product/broadcom-assy-top-bcm957414a4142cc-cloud-ethernet-netzwerkkarte-15730588)     | CHF 289.-       |
| Intel 82599ES 10Gbps SFP+ Dual Port PCI-E X520-DA2                                                                                                            | CHF 79.- (Used) |
| **Total**                                                                                                                                                     | **CHF 1363.50** |

## The installation

### Pulling the fiber

This was quite straight forward. Find a conduit that runs into the basement and push the pull line through. Then attach the fiber to it and pull it back very carefully.

![Fiber Pull](/images/P_20221024_175507.jpg)

### Terminating (splicing) the fiber

Although this was my first time splicing fiber it was quite straight forward and thanks to videos on youtube I was able to get a successful splice on the first try.

{{< youtube JP_C0XLLyR0 >}}

My splice setup in the basement where I had to terminate most Fibers.

![Splice Setup](/images/P_20221028_190926.jpg)

The distribution fiber is color coded. I terminated Blue and Red.

![Splice Setup](/images/P_20221028_213715.jpg)

Fiber stripped and cleaved ready for fusion.

![Fiber prepared for fusion](/images/P_20221027_200505.jpg)

Fusion

{{< youtube 1qaZZ_TcdnA >}}

The end result after heat-shrinking a protective sleave around the splice.

![Pig-tail attached](/images/P_20221028_214423.jpg)

### Basement splice box

To clean things up in the basement I added a [small splice box from aliexpress](https://www.aliexpress.com/item/32904850563.html) together with [APC Couplers](https://www.aliexpress.com/item/1005001493163074.html). Although not ideal it was good enough.

Very messy at the moment.

![Splice Box](/images/P_20221028_203246.jpg)

Incoming fibers also need some organizing.

![Splice Box](/images/P_20221028_203830.jpg)

### Outlets

In the end I had two different types of outlets. In some places I used these keystone plates and in one place I used the FTTH Squeeze one.

![Keystone outlet](/images/P_20221024_191019.jpg)

I did not want to disconnect the existing phone wiring (S-BUS) so I used a FTTH Squeeze OTO around the existing jack. This worked very well although my fiber routing needs some work. Some of those loops are close to the limit of this type of fiber.

![FTTH Squeeze OTO](/images/P_20221028_213117.jpg)

![FTTH Squeeze OTO](/images/P_20221028_220033.jpg)

In this case I also had to turn the FTTH Squeeze OTO 90 degrees because of the baseboard being in the way.

![FTTH Squeeze OTO](/images/P_20221028_220702.jpg)

![FTTH Squeeze OTO](/images/P_20221028_220841.jpg)

### PCs

On the PC side it is a simple PCIe card and SFP+ module. Patch coord I purchased from [FS](https://www.fs.com) and some from [Aliexpress](https://www.aliexpress.com/item/1005001615776579.html).

![PC](/images/P_20221021_212758.jpg)

![The Router](/images/P_20221029_140047.jpg)

## Conclusions

Generally I am very happy with how everything turned out but it did cost more than I wanted to spend.

### Speed tests.

Here are some initial tests I have run. I do still need to tune my router and proxmox/opnsense.

**Router to ISP**
[![Speedtest, pc to ISP](https://www.speedtest.net/result/c/b3548b67-bbf8-4b26-bfdd-21557c95ae57.png)](https://www.speedtest.net/result/c/b3548b67-bbf8-4b26-bfdd-21557c95ae57)

**PC to ISP**
[![Speedtest, Router to ISP](https://www.speedtest.net/result/c/c48584ab-f123-4648-85c8-952268bc00fb.png)](https://www.speedtest.net/result/c/c48584ab-f123-4648-85c8-952268bc00fb)

### Rough cost breakdown

| Part                        | Cost            |
| --------------------------- | --------------- |
| Router/Server               | CHF 1360.-      |
| Microtik Switches           | CHF 150.- (ea.) |
| Fiber (500m)                | CHF 170.-       |
| Fusion Splicer              | CHF 650.-       |
| 200 x Splice Sleeves        | CHF 12.-        |
| SFP+ Modules                | CHF 39.- (ea.)  |
| 12 x Pig-Tails              | CHF 12.-        |
| 10 x Patch Coords           | CHF 20.-        |
| Splice enclosure            | CHF 15.-        |
| APC Couplers                | CHF 15.-        |
| Keystone inserts            | CHF 8.-         |:
| Keystone wall-plate         | CHF 3.-         |
| FTTH Squeeze OTO wall-plate | CHF 34.-        |

### Would I do it again?

Yes :)

Comparing it to running CAT I would say it is very similar if not easier as with CAT my hands would usually be in pain the next day especially with CAT6 or CAT7. The fusion splicing is not difficult but requires some finesse and patience. You do loose the ability to do PoE but for that you have a future proof setup.
