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

featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png

tags: [""]
categories: ["blog"]

toc:
  auto: false
math:
  enable: true
---

My jounrey of getting my product manufactured in China and what I learned on the way.

<!--more-->

{{< admonition warning "Warning" true >}}
This is what I did however I am a complete amature when it comes to outsourcing manufcaturing and I went into this blind with a specific bufget I was willing to loose. There are probably much better ways to appreach this so take everything with a grain of salt. 
{{< /admonition >}}

## Introduction

I was on a crossroad with my product (Station Display) as I needed CE certification in order to sell them. Althought this is a self-ceritication you need to lab test your unit if use any RF (Wife/BLE etc.). The cost to do this in Switzerland is very high ($15k+) and for something I may not sell at all I did not want to make such a risky investment. 

One option would have been to sell the units un-assembled as a kit which puts the burden of CE on the assembling party but I didn't think that was the right thing to do.

So I decided to see if I could have some units made in China and have them CE certified there.

## The RFP

I had no idea where to start but I know I had to convey what I wanted in detail to whomever was going to manufacture my product.

To acheive this I spent a lot of time on a Request for proposal docuemnt. In it I included detailed design documentation and all the technical drawings required to build the unit.

I covered every details, from powder coat finish to the transmisibility of the front glass.

### Things to include
s
#### Quantaties

#### Design Files

#### Other

## Posting on Alibaba

Next was to upload the RFP on Alibaba. I created an RFQ on alibaba and uploaded a picture with a short description. I did not approach any specific companies althought I was looking into doing this if i did not get any good responses. Within a day or so I had about 10 companies requesting more information. I sent all except for a few the full RFP with all the files.

Important is to be truethfull upfront on how many units you are going to want. I had no idea what it would cost so I did not specify an budget. 

Also make sure you tell them you are not interested if you think it is not a good fit.

## Choosing a company

In order to evaluate the different companies I created a spreadsheet and cerafully tracked the communication. Specificlly I tracked what kind of questions they asked and how they responded to my answers.

One thing that made a difference was actually a mistake I had in the RFP. There was condradicting information in it and some companies caught it while others did not.

In the end I went by age of company, size and the interactions I had. I did not go with the cheapest. Out of all I had three that made a very good impression and understood what I was looking for. 

Also be aware that the person you are interacting with may not be too technical and how they deal with more technical issues is also important.

The two I did not pick I informed that I may in future want to use them as a backup manufactuere.


## Sending a sample

Some companies will ask for a sample or prototype in addition to the documentation. The company I went with did as well and I ended up sending them one. Expect the unit to be dissasembled and if you are still evaluating the company make some agreement for them to send the unit back if you don't choose them.

## First Sample run

After picking a company I had them make me an exact quote. This added for example the box and shipping as well.

Once I wired the full amount (normal for sample runs) they started with production.

Expect questions that can arrise during manufacturing as well compramizes that you may need to make. For example in my case we had an issue with the LED modules having a different mounting pattern than what the prototype had. I had to redesign the frame CAD and send it over. There was also an issue with the finish of the frame not being sandblasted (just painted) but a good manufucatuere will tell you and ask for your input. In my case the un-sandblasted looked good as well and we could proceed.

## Fixing more issues

Once you receive the units you will need to go through every box and check each unit. Since not every issue has the same severty I wrote up a small report for the manufacture. For example one critical issue was that the glass tint was too dark. This made the device unusable. A very minor issue was the packaging foam glue didn't stick correctly.

I went through every issue with the manufacture and we decided on the best course for correction in each case. In the case of the plexi-glass the manufacture had to find another vendor which did result in the doubling of the price for the plexi-glass. 

## CE Certification

CE Testing was also a requiremnt in my RFP. This is done by an outside testing facility but a good manufacture will have contacts and give you a quote for that as well.

This does require that you sacrifice several units. In my case 4 our of the 10 samples went to CE testing.

You will also need to fillout several forms for the CE testing facility and they may need to be able to reprogram your unit so having programing headers makes their life easier.

## Import duties

Switzerland has a free trade agreement with China but your manufactuere will need to specify the correct tarrif number as well possible provide a proof of origin.

You will of course still pay VAT like everything else you purchase for you business. 

## Second Sample run


