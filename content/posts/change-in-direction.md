---
title: "Change in Direction"
date: 2022-10-18T12:21:24-04:00
draft: false
---

When I originally conceived of my CARDIAC project, it had basically two phases. The first was to recreate my original project using Turbo C++, which I have completed. CARDIAC has most of the features that my original project had, and those that are missing are omitted intentionally, as I don't think they really fit. On the other hand, this version has some features that the original was missing. And I did all of this using Turbo C++, experiencing - or rather, re-experiencing - how C programming was done back in the day.

My second phase was to replace Turbo C++'s graphics library with my own, doing something I hadn't done back in the day - learn how to program directly to the VGA hardware. Unfortunately, this has hit a snag. Now that I'ved learned more about how VGA works, this goal doesn't quite make sense for me anymore.

# So what now?

I'm going to continue in the vein of DOS programming, but switching my focus to Mode 13h coding instead of 12h, as I'm doing with CARDIAC. My project will be a basic paint program. This will allow me to learn the basics of VGA Mode 13h programming, while at the same time starting to build up my own graphics library. And I'll have a tool I can use for developing my game.

I'm still focused on doing my work in DOS, thus I am limiting myself only to tools that will work in DOS. No cross-compilers or modern GUI editors for me! But I am not limiting myself to tools that were only available at the time, as I was with the CARDIAC project. So I can use any tool, whether it was developed in the 1982 or 2022, just so long as it runs under DOS.

With that in mind, I am looking at several different compilers:

- Turbo C++/Borland C++: This has the advantage of being familiar, but of course the compiler is 20+ years old.
- DJGPP: I've never used this before and there is some complexity to the setup, I'm finding. But it does have a nice editor, RHIDE, which is very similar to Turbo C's IDE. And it compiles DPMI-enabled software by default, so it gives me a 32-bit flat memory model, which is nice. But this may also be a downside, and is probably unnecessary since I doubt I'll write anything that requires more than the standard 640k. While this has been around for a long time, it has been updated and includes an updated GCC, so the version of C it can handle is much more modern.
- OpenWatcom: Like DJGPP, I've never used this before. And also like DJGPP, it's been around for a long time, being an updated version of the venerable Watcom C compiler. It doesn't compile DPMI-enabled code by default, but can use different DOS extenders, so DPMI and the 32-bit flat memory model that comes with it is available to me if I choose to go this route. This is the option I know the least about. I've played around with DJGPP just a little bit over the past day or two, but I haven't done anything with Watcom yet. But the DPMI flexibility issue and the fact that it comes in one package is causing me to lean in its direction.

# Why the change in direction?

Even before starting this project, I'd been considering writing a game for DOS. Back in the day, I only ever used Turbo C's graphics library for several projects, and then I moved on to Windows programming. So I didn't know much about the underlying hardware. So I'd hoped to use the second phase of my project to learn VGA programming to then apply to my future game.

The issue is that VGA has multple modes, two of which are relevant for this discussion: 12h and 13h. 12h is what I use for CARDIAC. It is VGA's highest-resolution mode at 640x480, but it's also limited to 16 colors. 13h's resolution is lower, 320x200, but it allows 256 colors. And the way one programs for these two modes is very, very different.

After doing the research on VGA, I realized that any game I'd want to write would probably use Mode 13h (256 colors) instead of Mode 12h (16 colors). This leaves me three options. First, I could rewrite CARDIAC using the lower-resolution 13h, but I don't think this would serve the project well. I think the lower resolution would really hurt the interface. Or, second, I could rewrite it using the higher-resolution 12h, but most of what I'd learn about VGA 12h programming wouldn't help me when I got to my game. Or, finally, I can call CARDIAC done and move on to what I really wanted to do from the beginning. Obviously, I'm choosing the last option.

# What about CARDIAC?

![CARDIAC 1.0](/IMG_1079.png)

It's pretty much feature-complete now. As I mentioned in an earlier blog post, there's a built-in delay when processing so as to allow the user to see what's going on. I've now added a way for the user to be able to adjust that delay or remove it entirely. To do this, I had to refactor how the delay is being done. Originally, I had it just call the "delay()" function, which just basically puts the machine to sleep for 1.5 seconds. This was easy to code obviously, but meant that it would be unresponsive during this delay. I changed this to a loop so that it could check for keyboard input while paused, thus making it much more responsive.

There are still a few minor things yet to do on the assembler, though while the features are minor I think they will cause me to refactor all my assembler lexing/parsing code. I did this all quickly without much forethought, and as any experienced programmer will tell you, that rarely results in great code. What I have now is functional, but not very flexible.

Finally, I want to build a DosBox bundled with CARDIAC. This would allow anyone curious enough to be able to easily download and run it.

But none of this is high on my priority list, which likely means it won't get done, especially since I doubt anyone will ever download and use it (feel free to let me know if I'm wrong).
