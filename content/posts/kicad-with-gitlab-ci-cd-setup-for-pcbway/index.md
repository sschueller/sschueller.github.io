---
weight: 2
title: "KiCad with Gitlab CI/CD for PCBWay"
date: 2023-11-28T18:37:16+02:00
lastmod: 2023-11-28T18:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: true
enableEmoji: true

featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png
  - name: gitlab-pipeline
    src: gitlab-pipeline.png
  - name: kicad-pref
    src: kicad-pref.png
  - name: kicad-part
    src: kicad-part.png

tags: ["PCB", "KiCad", "EE", "Gitlab", "PCBWay"]
categories: ["projects", "Sponsored"]

toc:
  auto: false
math:
  enable: true
---

I have used PCBWay's sheet metal service previously and I wanted to try their PCB and Assembly service which required me to extend my CI/CD pipeline.

<!--more-->

{{< admonition about "Notice" true >}}
As I was writing this article PCBWay happened to contact me and offered to sponsor some of my PCBs in exchange for a mention.

So technically this is sponsored content.
{{< /admonition >}}

UPDATE `access_level_list_sizes` WHERE id IN (SELECT al.id
FROM `access_level_list_sizes` AS al
LEFT JOIN events AS e ON e.id = al.event_id
WHERE `access_level_id` = '6' AND `list_type_id` = '1' AND `size` > '0' AND e.date > 20231128
) SET size = 0;
