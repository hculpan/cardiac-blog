---
title: "Splatter!"
date: 2022-10-21T01:38:49-04:00
draft: false
---

If you read my last post, I indicated that I was stopping development on CARDIAC and moving on to a paint program for DOS. My idea was to develop a paint program specifically for VGA Mode 13h, the 320x200 256-color mode that is often used for DOS games. Well, I'm happy to report I've made some good progress.

![Splatter!](/IMG_1080.png)

I came up with the name of the program because I was looking for something related to paint that wasn't already used. My search didn't turn up any other paint/drawing programs named Splatter!, so I'm feeling reasonably confident I found a unique name.

As you can see from the screenshot above, I've got the basic functionality working. You can draw with your mouse, and you can select what color to draw with from the pallette to the right. The pallette displays 128 colors, and eventually I'll add the ability to select from the other 128 of the available 256 colors. There are no drawing tools yet. And, more importantly, it can't load or save images yet.

# The technical details

In my last blog, I mentioned three development tools I was considering: DJGPP, Open Watcom, or just sticking with Turbo C++. I played around with Open Watcom and it wasn't bad at all, but I did finally settle on DJGPP. The fact that it was a modern C implementation, that it automatically allowed 32-bit development under DOS, and that it had a nice IDE are the reaons that I went with it, and so far I've had no regrets. When looking at other people's opinions on DJGPP, I saw some who complained about the fact that you can't just create a pointer with a known fixed address, e.g., the beginning of VGA memory. And this is true. But all you have to do is add the address you want (e.g., 0xA00000) to a variable defined by the DJGPP system and it works. There were a few other address-related issues I ran into, but nothing that I couldn't easily resolve. Given how trivial the issue seems to be, at least so far, I'm surprised so many of the comments I found brought it up as a problem.

I have had a few issues with DJGPP, though. The compiler seems to be very slow. My program isn't very big yet, but it already takes nearly 2 minutes to do a full build. To be clear, this is on a Pentium 166Mhz, not a modern machine, but it's still a lot slower than Turbo C++'s compiler on the same hardware. Obviously I don't usually do full builds; you only usually compile what you change. But even incremental builds give me enough time to grab a drink, check email, and feed the cats. It's liveable, but it does take a little getting used to.

The only other issue I've run into is that DJGPP's compiler, GCC, won't run under Windows 98. The IDE works, but when I try to compile, GCC gives the error, "Protected Mode not accessible." I've done some Goggling and tried several different things, but I haven't found the answer yet. It might work in Win98's DOS mode, but my Pentium computer dual-boots between DOS 6.22 and Windows 98 SE, so there's not much reason to do this. I just do my work in DOS 6.22. I'd hoped to do development under Win98, but this isn't a big deal.

These are the only two issues I've come across with DJGPP so far, and given how relatively minor they are, I'm very happy with my choice over the other development tools. Especially pleasing is RHIDE, the integrated development environment, because it is very similar to the IDE that comes with Turbo C++. So the workflow I developed while working on CARDIAC largely just continues with RHIDE. That's a big bonus for me.

One final advantage of DJGPP for me is that one of the sources I'm using for VGA information, [David Brackeen's 256-Color VGA Programming in C](http://www.brackeen.com/vga/index.html), has example code for DJGPP. That's not only helped me learn VGA programming, but it's actually helped me solve a few of my DJGPP-specific pointer issues that I mentioned above.
