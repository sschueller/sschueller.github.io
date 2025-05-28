---
weight: 2
title: "Manufacturing in China"
date: 2024-09-23T22:37:16+02:00
lastmod: 2024-09-23T22:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: true
enableEmoji: true

featuredImage: "header"
resources:
  - name: cover
    src: cover.png
  - name: box
    src: box.png
  - name: frame-dim
    src: frame-dim.png
  - name: label
    src: label.png
  - name: power-cable
    src: power-cable.png
  - name: ribbon
    src: ribbon.png
  - name: toc
    src: toc.png
  - name: zoll
    src: zoll.png   
  - name: header
    src: header.jpg   
  - name: assembly
    src: assembly.png   

tags: [""]
categories: ["blog"]

images:
  - '/posts/manufacturing-in-china/header.jpg'

toc:
  auto: false
math:
  enable: true
---

My journey of getting my product manufactured in China and what I learned along the way.

<!--more-->

{{< admonition warning "Warning" true >}}
This is what I did; however, I am a complete amateur when it comes to outsourcing manufacturing and I went into this blind with a specific budget I was willing to lose. There are probably much better ways to approach this, so take everything with a grain of salt.
{{< /admonition >}}

## Introduction

I was at a crossroad with my product (Station Display) as I needed CE certification in order to sell them. Although this is a self-certification, you need to lab test your unit if you use any RF (WiFi/BLE etc.). The cost to do this in Switzerland is very high ($15k+) and for something I may not sell at all, I did not want to make such a risky investment.

One option would have been to sell the units unassembled as a kit, which puts the burden of CE on the assembling party, but I didn't think that was the right thing to do.

So I decided to see if I could have some units made in China and have them CE certified there.

## The RFP

![RFP Cover Page](cover)

I had no idea where to start but I knew I had to convey what I wanted in detail to whomever was going to manufacture my product.

To acheive this I spent a lot of time on a RFP (request for proposal) docuemnt. In it I included detailed design documentation and all the technical drawings required to build the unit.

I covered every details, from powder coat finish to the transmisibility of the front glass.

### Things to include

This was my Table of Contents of the RFP.

![RFP Table of Contents](toc)

#### Change Log

I Added this so the manufacturer can easily see what I changed between versions I sent them. On the bottom there is a Revision number that is autoatmicly updated when I save the document in OpenOffice.

#### Overview

The overview includes a summary of what I want which includes pricing for x amount of units as well as time frames for x amounts of units etc.

#### Scope of Work

Here is all the detail of each part and what the manufacturer needs to do

I went into a lot of detail so I even drew the box in CAD and the foam that would go into it

![Box](box)

Even the product labeling has to be defined or it will end up somewhere where you may not want it to be. I also specified the material of the label and the size of course.

For things like this, if you don't know what to use you can always ask the manufacturer what others use or what they recommend. You can write in the RFP that they should propose a solution.

![CE Label and placement](label)

For the assembly I created a guide in FreeCAD using a [plugin](https://osh-autodoc.org/). I spent way too much time on this...

![Assembly Manual](assembly)


## Posting on Alibaba

Next was to upload the RFP on Alibaba. I created an RFQ on Alibaba and uploaded a picture with a short description. I did not approach any specific companies although I was looking into doing this if I did not get any good responses. Within a day or so, I had about 20 companies requesting more information. I sent all except for a few the full RFP with all the files.

Important is to be truthful upfront on how many units you are going to want. I had no idea what it would cost so I did not specify a budget.

Also make sure you tell them you are not interested if you think it is not a good fit.

## Choosing a company

In order to evaluate the different companies I created a spreadsheet and carefully tracked the communication. Specifically I tracked what kind of questions they asked and how they responded to my answers.

One thing that made a difference was actually a mistake I had in the RFP. There was contradicting information in it and some companies caught it while others did not.

In the end I went by age of company, size and the interactions I had. I did not go with the cheapest. Out of all I had three that made a very good impression and understood what I was looking for. 

Also be aware that the person you are interacting with may not be too technical and how they deal with more technical issues is also important. Do they pass the question on to a technical person etc.

The two I did not pick I informed that I may in future want to use them as a backup manufactuere. This is also a standard question they will ask you if you tell them you picked someone else.


## Sending a sample

Some companies will ask for a sample or prototype in addition to the documentation. The company I went with did as well, and I ended up sending them one. Expect the unit to be disassembled, and if you are still evaluating the company, make some agreement for them to send the unit back if you need it back.

## First Sample run

After picking a company I had them make me an exact quote. This added for example the box and shipping as well.

Once I wired the full amount (normal for sample runs) they started with production.

Expect questions that can arrise during manufacturing as well compromizes that you may need to make. For example in my case we had an issue with the LED modules having a different mounting pattern than what the prototype had. I had to redesign the frame CAD and send it over. There was also an issue with the finish of the frame not being sand blasted (just painted) but a good manufucatuere will tell you and ask for your input. In my case the un-sand blasted looked good as well and we could proceed.

## Fixing more issues

Once you receive the units you will need to go through every box and check each unit. Since not every issue has the same severty I wrote up a small report for the manufacture. For example one critical issue was that the glass tint was too dark. This made the device unusable. A very minor issue was the packaging foam glue didn't stick correctly.

I went through every issue with the manufacturer and we decided on the best course for correction in each case. In the case of the plexi-glass the manufacturer had to find another vendor which did result in the doubling of the price for the plexi-glass. 

## CE Certification

CE Testing was also a requirement in my RFP. This is done by an outside testing facility, but a good manufacturer will have contacts and give you a quote for that as well.

This does require that you sacrifice several units. In my case 4 out of the 10 samples went to CE testing.

You will also need to fill out several forms for the CE testing facility and they may need to be able to reprogram your unit, so having programming headers makes their life easier.
## Import duties

Switzerland has a free trade agreement with China but your manufactuere will need to specify the correct tarrif number as well possible provide a proof of origin.

You will of course still pay VAT like everything else you purchase for you business. 

## Second Sample run

For the second sample run I
