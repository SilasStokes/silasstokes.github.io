---
layout: post
title: wigglegramify your quadrascopic photos
date: 2023-06-20 14:18
category: 
author: 
tags: []
summary: I am launching wigglegramify.com, a website that allows you to turn your quadrascopic photos (from nishika or nimslo cameras) into gifs or videos.
---

## tl;dr 
check out the website I just launched at [www.wigglegramify.com](https://www.wigglegramify.com)\*. 

\*\* but beware of the current UI, which I talk about below

## I have a problem...

In classic cart-in-front-of-horse-fashion I bought a nishika quadrascopic 35mm film camera, shot 3 rolls of film at a wedding, and now I have no way of creating the fun wiggle effect for my friends. 

If you're unfamiliar with the camera, here's a picture of it
![Nishika N8000](/assets/images/2023-06-20-launch-of-wigglegramify/nishika-n8000.jpg)

The camera has 4 lenses (hence the quadrascopic), each of them exposes half of a 35mm frame in the same instant. Once you get photos developed, you can create a slideshow out of the 4 images it takes and it creates the wiggle effect you can see here:

![Nishika Example gif/mp4](/assets/images/2023-06-20-launch-of-wigglegramify/example.mp4)

I used to do this effect with [Photoshop](https://stereoscopy.blog/2020/06/21/making-wigglegrams-using-photoshop-tutorial/) but I stopped paying for my adobe subscription years ago. The easiest thing to do would have been to restart my subscription but I have literally no other use for photoshop. Armed with the negatives from the wedding, I started looking around to see if there was any other ways to make it that doesn't require special software. I found some cool projects made in the r/wigglegrams subreddit (currently locked due to the great Reddit strike), like [this one that creates the effect using Python](https://github.com/crocagiles/wigglegrams). But it was clunky, iirc I had to edit some of the code to make it functional. I was genuinely surprised that there was no web app dedicated to this process considering how much buzz exists for wigglegrams. Seeing as I am working on my web dev journey, I figure it was a sign to give it a try. 

[And it turns out I was actually able to get it working](https://www.wigglegramify.com)

The site is live but extremely alpha. I am fairly certain any photos except the test set I programmed it alongside will break it so perhaps just bookmark it and come back to it in 2 weeks. The site is written in Typescript using React and Next.js and I deployed on Vercel.  

The UI is intentionally lacking, I did as little as possible so I could focus on the technical aspects of how to layer the photos and then gif-ifying them. It turns that this little project is one of the beneficiaries of the push towards WASM. The elephant in the room when I started the site was how was I going to take the 4 images and turn them into a video. I knew that it could be easily done by running a backend but I was hoping that I could figure out a way around it. Enter [ffmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm). I was honestly shocked at how easily it was to get working in my project. I trial and error'd in my command line for a few minutes to find the right flags to pass to it, then just translated that to the `ffmpeg.run(...)` the package provides and out popped my first gif from the wedding. 

![gif from the wedding](/assets/images/2023-06-20-launch-of-wigglegramify/out.mp4)

With `ffempeg.wasm`, the entire functionality of the site is clientside so the website is serverless which I am very happy about :-\). To finish off my first iteration all I had to do was work on the scaling, cropping and layering in the UI. 

I am very happy about creating something that has utility. Having a project that solves a problem unconsciously makes the process so much easier. I am excited to keep working on it!

The next steps for this project as I see it is:
1. make the logic and error handling more robust so I can move the functionality to beta.
2. re-write and consolidate all the packages I used for drag and drop, (why did I use 3 separated draggable npm packages??).
3. redesign the UI inspired by the GUI's in evangelion.

I will update as I make progress, so check in sometime soon! 

