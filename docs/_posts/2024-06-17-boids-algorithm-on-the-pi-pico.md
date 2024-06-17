---
layout: post
title: boids algorithm on pi pico
date: 2024-06-17 05:57
category: project
author: silas stokes
tags: ["raspberry pi pico"]
summary: I implemented boids algorithm on the pi pico with an ILI9341 display, designed an enclosure and added a controlling encoder.
---

I have been exploring the raspberry pi pico and it's rp2040 processor quite a bit. I came across the lectures from Cornell's Digital Systems Design Using Microcontrollers where the instructor assigns a project to implement boid's algorithm on the pico. I thought that if designed right - it would look like a very cool desk piece. Here's what I came up with:

<iframe width="1280" height="932" src="https://www.youtube.com/embed/2z1mezkGZ8A" title="boids algo on rpi2040 using ili9341 and an encoder" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin"></iframe>

Since I did use code from the cornell course - I am opting to not post the source code. Some details about it though: 
- I use one core for drawing the screen and reading encoder input; the other core is doing the boid calculations.
- The course gives a psuedo threading library called "protothreads" by Adam Dunkel which I found fascinanting. It essentially leverages c preprocessor statements to wrap function internals with switch statements so you can maintain your place in the function after yielding. Neato. 

Once I had the firmware working - I had to design an enclosure:
![Screenshot of Fusion360 with the enclosure design](/assets/images/2024-06-17-boids-algorithm-on-the-pi-pico/enclosure-cad-design.jpeg)