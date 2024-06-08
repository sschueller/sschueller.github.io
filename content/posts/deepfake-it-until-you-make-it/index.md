---
weight: 2
title: "Deepfake it until you make it"
date: 2024-01-12T22:37:16+02:00
lastmod: 2024-01-12T22:37:16+02:00
author: "Stefan Schüller"
authorLink: "https://github.com/sschueller/"
description: ""
draft: true
enableEmoji: true

featuredImage: "rendered-esp32-base"
resources:
  - name: rendered-esp32-base
    src: render.png

tags: ["StableDiffusion", "AI", "Instagram", "DeepFake"]
categories: ["blog"]

toc:
  auto: false
math:
  enable: true
---

I have been seeing a lot of news articles regarding "AI instagram models" that are raking in large amounts of money and fooling famous footballers to slide into their DMs. So that sparked my curiosity as to what exactly is going on because from my experience with AI, we aren't there yet. So how exactly are they doing this?

<!--more-->

> Many thanks to those posting their workflows at [r/StableDiffusion/](https://old.reddit.com/r/StableDiffusion/) . By sharing we learn from each other and improve upon all work.

{{< admonition warning "Warning" true >}}
I am not an expert when it comes to AI. There may be better/faster ways to do anything I describe. Feel free to leave me a comment below.
{{< /admonition >}}

## Emily Pellegrini

Of course from news sites you can't get any details as they are in a race to get the most views for the least amount of work... Instead they make it sound like the whole thing is done by AI. 

So I decided to take a closer look at the one talked about the most in European papers at this time which is Emily Pellegrini. The images aren't interesting to me because those can be easily generated with stable diffusion on any PC with a decent GFX card. However the videos are not something that can't be so easily generated to look realistic. Especially anthing over a few seconds long.

Looking at Emily Pellegrini's videos you can quickly see that there appears to be a faceswap going on as it's not cleanly done. A faceswap would mean the videos are real and the only possible AI is someone swaping someone elses face. Most likely an AI generated face. Imidately I was aksing myself, why would someone swap a face when the rest is real? Well I solved that ridle in only a few minutes. I ran one of the photos through a reverse image search and found the original foto which belongs to Lyna Perez and of course only the face was altered. 

So in conclusion, there is very little AI, not even the photographs are full AI. Someone merely stole someone elses images/videos and did a face swap. What a disapointment.

## What can actually done with AI

Here is what I know can be done with some examples. Much more can be found at [r/StableDiffusion/](https://old.reddit.com/r/StableDiffusion/) or at [Civitai](https://civitai.com/)

### Photographs

As I mentioned above, generating still images in the style of instagram is easy and can be done on comsumer hardware. Here is a workflow in ComfyUI which is a UI for StableDiffusion. On a RTX 3080 Ti these can be generate in a few seconds.

### Videos

Videos are another level. There are two methods here which both have pros and cons.

#### Stable Video Diffusion

SVD was release only a little over a month ago. With SVD the length of the video is very limited. 14 and 25 frames at customizable frame rates between 3 and 30 frames per second. 

#### ControlNet and AnimateDiff

The process here is to use an existing video to generate a controlnet which is then used to render Ai images. These are then stiched together and animatediff is used to make the transition between images smoother.

The resulting video is clearly AI generated as it has a morphing effect due to the fact that consitence between images is not 100%. 


