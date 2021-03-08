---
title: "Attraction"
date: 2021-03-08T19:00:35+05:30
showDate: true
tags: ["blog","story"]
---

And we are back. It has been 2 years, a lot has changed, except for my generative art skills. Still learning the ropes.
I started looking into attractors but got distracted and rather started watching [The Coding Train's Gravitational Force video](https://www.youtube.com/watch?v=fML1KpvvQTc).

I prepared a similar setup, and the idea was to add a lot of objects and see if it produces anything interesting. No dice.

Of course, just because there is a lot of flying stuff doesn't mean it's "art", I said to myself. The next idea was again straight out of
generative playbook. Map one dimension to a completely different, utterly unrelated dimension. 

And that worked!

## Process

So we have a 'attractor' which applies gravitational force on 'movers'. This force(which is a 3d vector), then changes the movers' acceleration, which in turn changes its velocity,
which in turn changes its position(a 3d vector again).
And if we were to update each mover's position according to applied force we'd have a flying mess.

But what if we keep the mover stationary(in space) but update its color rather(moving in RGB space). We have 3 dimensions for color(r,g,b) and we have a 3d position vector, seems fitting.

## Issues 

I was working with mere 50k particles, and if each particle was to be treated like a pixel, those are not a lot of pixels.

I tried a few tricks, and only one worked. So we have 50k particles(pixels) forming a square, let's take a few and shift them in space, but only odd pixels need to be shifted.
This yield smaller tiles, but since they are more than 1 and take more space than a 50k cross square, it looks bigger.

And here we have the results! Each one looks like something to me, so I have added those comments as well, while trying to sound as artistic as possible.

---
## Eternal Disco Ball
![](/art/gallery/post-images/c_anim_3.gif)

---
## Duality

![](/art/gallery/post-images/c_anim_5.gif)

---
## Sunless Sky

![](/art/gallery/post-images/c_anim_6.gif)

---
## Doesn't look like anything to me

![](/art/gallery/post-images/c_anim_7.gif)

---
## Ripple

![](/art/gallery/post-images/c_anim_8.gif)

---
## Cause and Effect

![](/art/gallery/post-images/c_anim_9.gif)

---
## Wednesday, Thursday, Friday, Saturday

![](/art/gallery/post-images/c_anim_12.gif)

---
## Entanglement

![](/art/gallery/post-images/c_anim_13.gif)

---
## Revenge of the Ripple

![](/art/gallery/post-images/c_anim_14.gif)

---
## Cosmic Butthole

![](/art/gallery/post-images/c_anim_17.gif)

---
## Fucking global warming

![](/art/gallery/post-images/c_anim_18.gif)

---
## Flume

![](/art/gallery/post-images/c_anim_22.gif)

---
## Disintegrate

![](/art/gallery/post-images/c_anim_23.gif)

---