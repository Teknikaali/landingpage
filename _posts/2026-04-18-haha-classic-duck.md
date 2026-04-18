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

![][0]

## The premise

Using emojis is part of my style of messaging during the workday. They're quite a nice way to express yourself a little bit more than just typing words. Of course, there is time and place when one shouldn't use them, but for e.g. in-team discussions, I've found them quite neat.

The "Haha, classic" duck emoji is one of the custom emojis that gets used by me and other coworkers every now and then:

![][1]{: .center-image }
<figcaption class="caption">The original "Haha, classic" duck</figcaption>

It was created by Egor Zhgun in 2019 as a sticker pack for Telegram called "[Utya Duck Telegram Stickers](https://www.behance.net/gallery/93658133/Utya-Duck-Telegram-Stickers#)".

The mouth movement, shaking of the head, and the expression on the duck's face are just \*chef kiss\*. But it has one problem that has irritated me for a while now.

## The problem

The duck has a smoking cigarette in its hand.

I don't smoke and I don't advocate smoking, because it is [bad for your health](https://thl.fi/en/topics/alcohol-tobacco-and-addictions/tobacco) and for people around you.

For what's it worth, I feel like we should stop giving space and screen time for things that are hurting us deliberately. But as humans are humans, these things still exist because of reasons unbeknownst to me.

I tried searching for alternative versions, but I couldn't find anything that would please me.

[Tenor](https://tenor.com/search/haha-classic-duck-gifs) had quite the selection, but nothing caught my eye as "the solution". There was always something wrong with the animation e.g. unnecessary added clothing, accessories, or the gif just wasn't suitable for actual emoji usage: file size too big, too many frames, missing transparency, bad aspect ratio... you name it.

![][2]{: .center-image }
<figcaption class="caption">Unnecessary accessories</figcaption>

## The solution

With an idea in my head, some tools and too much time on my hands one late evening, I decided to create something that would tick all the boxes:

- Aspect ratio 1:1
- Max. 50 frames of animation for compatibility: some apps, like Slack, limits this to max. 50
- Transparent background: Must work well in dark/light mode
- The cigarette is replaced with a coffee cup
- The cup moves correctly in the duck's hand and has more pleasant "smoke" color

## Tools used

- [Paint.Net](https://www.getpaint.net/): There are many image and photo editing programs out there, but this is the one I'm most comfortable with. It doesn't have support for animated gifs out-of-the-box, but there should be relevant plugins available.
- [ImageMagick](https://imagemagick.org): This tool could do so much more. If you ever have to do some image processing e.g. crop/resize/format conversion during runtime in your app, this is the tool to look out for. I only used the CLI.
- [Ezgif](https://ezgif.com/): Surprisingly, a very handy online gif editing tool for things like cropping, resizing and optimizing the final output. I just happened to search for "gif tool" and it was the first search result. (I wonder why I just didn't use [ScreenToGif](https://www.screentogif.com/) and its gif editor I'm familiar with. Oh well.)

## The progress

The whole project took me around 4 hours to finish (the time to write this post is not included).

The most time-consuming step was bringing the duck's hand in front of the coffee cup. Editing each frame of the 50 was a bit cumbersome, but doable.

Before I could work with the hand part, I had to cut the cigarette and the smoke to their own layer to make things easier. It went smoothly by using the Magic Wand tool with quite specific Tolerance setting as it had different colors than the duck. For some other gif this could be a whole different story.

For each frame I selected the part of the hand that was going to overlap the cup, copied it to a new layer and saved the image as .pdn (Paint.Net format) to preserve the layers. Sometimes the selection I did took too much of the area outside the hand's border line, but it was easily fixed.

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

I had to make some minor corrections to the frames here and there. The original animation had a black solid background that had to be converted into transparent. This caused the eyes to disappear. Some pixels were missing, so I had to go and add those in before using the fill tool for the eyes. Now that I think of it, I might have taken the wrong source file. Transparency was easy to do with Ezgif.

Some small touch-ups had to be done in frames 46 and 47 because the "haha, classic" text had its letters move vertically in sync with the mouth, partly covering the smoke.

I also had to take out one "head bobbing" cycle to fit the animation into 50 frames. There were a total of 55 frames, so 5 had to go. Losing one cycle was a small sacrifice I was willing to take without losing too much of the charm of the original animation.

![][7]{: .center-image }
<figcaption class="caption">Touch-ups</figcaption>

Emojis are quite small. Even smaller, if you don't crop the canvas correctly and leave too much white space around the thing we're interested in.

I used [ImageMagick](https://imagemagick.org) to do a batch crop job to make sure to only fit in the animation frames what we're really interested in: a duck who babbles about a classic thing.

This is the script I ended up using.

```
magick.exe mogrify -path ./cropped -crop 472x472+7+40 ./source/*.png
```

I had to add an empty `cropped` folder and put all the .png source frames into `source` folder for this to work.

## Image and file size

Animated gifs used as emojis should be small in file size and mostly they are viewed in very small sizes on-screen. For example Slack has 128 KB strict file size limit and emojis are displayed at around ~22-32px.

After cropping was done I used Ezgif to resize the gif to 128x128 and then fiddled with the optimization setting until I got the file size the desired size limit.

## The final result

![][8]{: .center-image }
<figcaption class="caption">"Haha, classic" duck emoji, cigarette replaced with a coffee cup, 128x128</figcaption>

## Afterthoughts

Was this, in any way, a sensible project to take on? Not at all.

Who would benefit reading this? I have no slightest idea.

Was it still worth it? Absolutely.

### Things that maybe should have been done differently:

- Take your time finding as high resolution version as you can of the gif you want to make modifications to. This way you have less artifacts to fight with.
- Take a note if the animation's background is black or white, if you can't get a transparent one. It might affect how semi-transparent objects (or in my case: the steam) looks like.
  - ![][9]
  - You can also notice how the eyes are not as sharp on the right one (my version) as they're on the left one.
- [Apparently](https://graphicdesign.stackexchange.com/questions/113306/why-does-enabling-transparency-in-an-animated-gif-reduce-its-file-size), _enabling_ transparency when exporting a gif _reduces_ its file size. I got curious about this after realizing the smoke wasn't transparent in the source gif I used. I used the gif with the dark background available at [KnowYourMeme](https://knowyourmeme.com/memes/duck-smoking-gif).
- Acquire the original source animation somehow so you could use the same (or similar) tooling the original author used. This is nigh impossible task I reckon. Would the original author make the source file available for use? Hardly.
- Take a moment to look at the problem at hand (figuratively and literally) first before you put your hands in the clay. Would it have been easier to cut out a piece out of the coffee cup instead of bringing the hand in front of it? Maybe. But then I might have had to rotate the coffee cup at one point of the animation and handle _coffee physics_ inside the cup, so maybe the path I took was OK.
- Use something else than Paint.Net for the editing task. It's a good piece of software, don't get me wrong, but just maybe not the most efficient tool for the job. Maybe one could have set up some vector drawing program or an actual gif editing program to do the job faster and more precise. Animation tweening tool would have helped a lot.
- Could this kind of "simple" animation editing be done with an AI tool in a jiffy? Maybe.

## The end

Now go, and add the emoji to your workplace's ever-growing emoji collection, and enjoy the "haha, classic" moments in your life without having to think about cigarettes, smoking and their negative impact on life every time something classic happens!

I just might have done that.

[0]: /assets/images/classic-coffee/classic-coffee-header.jpg
[1]: /assets/images/classic-coffee/original-classic-128x128.gif
[2]: /assets/images/classic-coffee/unnecessary-accessories.jpg
[3]: /assets/images/classic-coffee/making-of-select-hand.jpg
[4]: /assets/images/classic-coffee/making-of-steam.jpg
[5]: /assets/images/classic-coffee/making-of-dents.jpg
[6]: /assets/images/classic-coffee/making-of-dents.gif
[7]: /assets/images/classic-coffee/touch-ups.jpg
[8]: /assets/images/classic-coffee/classic-coffee-128x128.gif
[9]: /assets/images/classic-coffee/light-dark-difference.jpg