---
layout: post
title: Lighting a resin pour with pi pico and micropython
date: 2024-04-25 11:15
category: python programming
author: silas stokes
tags: []
summary: 
---

Wanted to share another little project I did these last few weeks. Made it as a gift for my dear friends who got married. It is a resin pour of flowers from their wedding. I got to use micropython and the raspberry pi pico which I have been wanting to do for some time. 

Here's a video of the final project.

[Here](https://youtube.com/shorts/cUoQCN8cpLM?feature=share)

First I had to pour the epoxy. I got a random large silicon mold off of amazon, about 5" square and 2" deep and some SuperClear Liquid Glass Resin. The plan was to pour one layer, put the flowers in it, then after it's partially solified, pour the second.

![image of epoxy being mixed in cup](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/mixing_epoxy.jpeg)

Unfortunately I poured the second layer too soon (I did wait 30 hours) and the flowers floated out of the bottom layer and protuded beyond the top of the mold. I 3D printed a skirt that I put in the mold to try to get a third layer on top of the floating flowers (the brown brim in the picture). It ended up being a whole mess because the epoxy slowly leaked under the brim and out of the silicon mold.

![image of flowers floating above brim of silicon mold](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/floating_flowers.jpeg)

By the time I was done, the epoxy just ~barely~ covered the flowers and the nice crisp edges were ruined. The flowers looked good though.

![image of flowers in an epoxy mold](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/resin_done.jpeg)

I spent a whole Saturday morning sanding down the edges. I used disc sander but had I known how long it would take up front, I probably would have bought a belt sander just for this task. Here is it with the edges sanded. 

![image of the resin block with the edges sanded](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/sanded.jpeg)

I was bummed about the edges but I realized that the opaque sides of the pour would probably diffuse light so I decided to add making a frame with LEDS to the project. I designed the frame in Fusion360. I had some left over NeoPixels from building [dancing bear project](ADD LINK) so that is what I used as the leds. I don't have any good pictures of the case so here's two pictures of me running the hardware in it. 

Here's running the wires for the encoder:
![image of the frame with encoder wires running in the inner trough](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/encoder.jpeg)

And the kludge I had to do when the Neopixel didn't have enough clearance to go over the encoder (whoops):
![image of the frames inner trough with led strip cut and wires ran over an encoder](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/soldering.jpeg)

Finally I had to write some code. My first time using MicroPython. I was pleasantly suprised at how easy it was. Granted my use case was extremely minimal but I was able to get the whole thing up and running in an afternoon when I had a blocked out a weekend. I'd add the code to the post (or github) but the MicroPython development environment (Thonny) does a remote file server situation so when I went to open Thonny and get my files for this blog, I realized that they were on the pico that I've already given to my friends. 

Here's me doing some test breadboarding:
![image of the frames inner trough with led strip cut and wires ran over an encoder](/assets/images/2024-04-25-raspberry-pi-pico-and-epoxy-pours/test_wiring.jpeg)

I don't have any videos of all the effects I programmed which is another bummer. I also can't take any credit for the effect in video I linked at the top as the gradient is the first example in the pico neopixel library. Lesson learned however to document more during the project rather than trying to go back after. 

Thanks for reading!
