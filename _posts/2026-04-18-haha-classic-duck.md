---
title: "Smoke-free classic duck"
image: assets/images/classic-coffee/classic-coffee-header.jpg
layout: post
date: 2026-04-18 10:00
tag:
- creative
category: blog
author: anssikettunen
description: Reimagining the classic duck gif as smoke-free and compatible with chat apps
---

# Smoke-free classic duck

## The premise

Emojis are a small part of my day-to-day work. They're quite a nice way to express yourself a little bit more than just typing words in chat apps and the like. Of course, there is time and place when one shouldn't use them, but for in-team discussions, I've found them quite neat.

Here is one custom animated emoji that gets used every now and then: the "Haha, classic" duck:

![][1]{: .center-image }
<figcaption class="caption">The original "Haha, classic" duck</figcaption>

The mouth movement, shaking of the head, and the expression on the duck's face are just \*chef kiss\*. But it has one problem that has irritated me for a while now.

## The problem

I don't smoke and I don't advocate smoking, because it is [bad for your health](https://www.cdc.gov/tobacco/about/index.html) and for people around you.

The duck has a smoking cigarette in its hand. I don't need to be reminded of the act of smoking every time nor see that each day, thank you.

I tried searching for alternative versions, but I couldn't find anything that would please me.

[Tenor](https://tenor.com/search/haha-classic-duck-gifs) had quite the selection, but nothing caught my eye as "the solution". There was always something wrong with the animation e.g. unnecessary added clothing, accessories, or the gif just wasn't suitable for messaging app emoji usage: file size too big, too many frames, missing transparency, bad aspect ratio... you name it.

![][2]{: .center-image }
<figcaption class="caption">Unnecessary accessories</figcaption>

## The solution

With an idea in my head, some tools and too much time on my hands one late evening, I decided to create something that would tick all the boxes:

- Aspect ratio 1:1
- Max. 50 frames of animation for compatibility: some chat apps, like Slack, limits this to max. 50
- Transparent background: Must work well both in dark mode and ohmygodwhoburnedmyeyes mode
- The coffee cup should move in the duck's hand and have a bit more pleasant "smoke" color

## The progress

I had to take out one "head bobbing" cycle to fit the animation into 50 frames. There were a total of 55 frames, so 5 had to go. Losing one cycle was a small sacrifice I was willing to take without losing too much of the charm of the original animation.

The most time-consuming part was bringing the duck's hand inf front of the coffee cup. Editing each frame of the 50 was a bit cumbersome, but doable.

![][3]{: .center-image }
<figcaption class="caption">Bringing the hand in front of the cup</figcaption>

I didn't want the warm steam rising from the coffee remind too much of the smoke. First I recolored the gray smoke to something a bit warmer. Since coffee is brown, of course the animated steam should also be brown (if you squint your eyes a little bit).

![][4]{: .center-image }
<figcaption class="caption">Recoloring the smoke</figcaption>

There was a bit of uncertainty about whether I'd like to keep the original smoke-like shape of the steam, but in the end, I decided to keep it so I wouldn't deviate too much from the original work. In the process, I found out the Distort -> Dents effect tool could have made quite a nice steam-looking effect:

![][5]{: .center-image }
<figcaption class="caption">Denting the steam</figcaption>

![][6]{: .center-image }
<figcaption class="caption">What-it-could-have-been</figcaption>

I had to make some minor corrections to the frames here and there. The animation had originally a black solid background that had to be converted into transparent. This caused the eyes to disappear. Some pixels were missing, so I had to go and add those in before using the fill tool for the eyes.

Some small touch-ups had to be done in frames 46 and 47 because the "haha, classic" text had its letters move vertically in sync with the mouth, partly covering the smoke.

![][7]{: .center-image }
<figcaption class="caption">Touch-ups</figcaption>

Emojis are quite small. Even smaller, if you don't crop the canvas correctly and leave too much white space around the thing we're interested in.

I used [ImageMagick](https://imagemagick.org) to do a batch crop job to make sure to only fit in the animation frames what we're really interested in: a duck who babbles about a classic thing.

This is the script I ended up using.

```
magick.exe mogrify -path ./cropped -crop 472x472+7+40 ./source/*.png
```

I had to add an empty `cropped` folder and put all the .png source frames into `source` folder for this to work.

## Tools used

- [Paint.Net](https://www.getpaint.net/): There are many image and photo editing programs out there, but this is the one I'm most comfortable with. It doesn't have support for animated gifs out-of-the-box, but there should be relevant plugins available. Ezgif was sufficient enough for my needs.
- [ImageMagick](https://imagemagick.org): This tool could do so much more. If you ever have to do some image processing e.g. crop/resize/format conversion during runtime in your app, this is the tool to look out for. I only used the CLI.
- [Ezgif](https://ezgif.com/): a very handy online gif editing tool for things like cropping, resizing and optimizing the final output.

## The final result

![][8]{: .center-image }
<figcaption class="caption">"Haha, classic" duck emoji, cigarette replaced with a coffee cup, 128x128</figcaption>

## Afterthoughts

Was this, in any way, a sensible project to take on? Not at all.

Was it worth it? Absolutely.

Things that maybe should have been done differently:

- Acquire the original source animation somehow so you could use the same (or similar) tooling the original author used. This is nigh impossible task I reckon. Would the original author really make the source file available for use? Hardly. Who really knows who makes all these gifs anyway? I wouldn't even know where to start looking because of "[I made this](https://knowyourmeme.com/memes/i-made-this)".
- Use something else than Paint.Net for the editing part. It's a good piece of software, don't get me wrong, but just maybe not the most efficient tool for the job. Maybe one could have set up some vector drawing program or an actual gif editing program to do the job faster and more precise. Animation tweening tool would have helped a lot.
- Maybe don't go out of your way and write a huge blog post about how you edited some random gif you found out a little bit irritating in your day-to-day life. Do something more meaningful with your life. Call your loved ones and tell them you love them or something.

Now go and add the emoji to your workplace's ever-growing emoji collection, and enjoy the "haha, classic" moments in your life without having to think about smoking!

[1]: /assets/images/classic-coffee/original-classic-128x128.gif
[2]: /assets/images/classic-coffee/unnecessary-accessories.jpg
[3]: /assets/images/classic-coffee/making-of-select-hand.jpg
[4]: /assets/images/classic-coffee/making-of-steam.jpg
[5]: /assets/images/classic-coffee/making-of-dents.jpg
[6]: /assets/images/classic-coffee/making-of-dents.gif
[7]: /assets/images/classic-coffee/touch-ups.jpg
[8]: /assets/images/classic-coffee/classic-coffee-128x128.gif