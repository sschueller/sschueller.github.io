---
weight: 2
title: "Upgrading cheap blinds into IoT"
date: 2023-12-31T17:37:16+02:00
lastmod: 2023-12-31T17:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: true
enableEmoji: true

featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png

tags: ["PCB", "EE", "Project", "PCBWay"]
categories: ["projects"]

toc:
  auto: false
math:
  enable: true
---

How I turned some cheap windows blinds into IoT controllable electronic ones.

<!--more-->

{{< admonition about "Notice" true >}}
This post is sponsored by PCBWay and was entered in the PCBWay 6th Project Design competition.
{{< /admonition >}}

## Introduction

I needed some blinds for the window in front of my desk so i found these (cheap)[https://www.obi.ch/rollos-raffrollos/gardinia-easyfix-rollo-uni-hellgrau-struktur-150-x-45-cm/p/1799493] ones at the local hardware store. They work great but I requires me to climb over my desk top operate them. So I deced to go down the rabbit hole to convert them to electronic shades...

## Research

Since these types of blinds are quite common I assumed I would find some existing project on thingiverse or other site. Indeed I found a few projects that control similar type shades with chain but nothing really fit for what I wanted.

## Motors & Controllers
