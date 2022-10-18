---
title: "Hello world!"
date: 2022-10-02T00:45:51-04:00
draft: false
---
Before I talk about my first day of coding, I wanted to give a brief overview of the tools I'll be using.  As I've said in my previous posts, all development will be done in MS-DOS, specifically version 6.22.  I'll be using Borland's Turbo C++ 3.0, both the IDE for coding and debugging, and the compiler for building the executable.  I don't anticipate using any other DOS tool to any significant degree.

Other tools I'll use will include Github to host the code, copied from my DOS machine to my Mac and uploaded to Github from there.  And, of course, I'll be using my Mac to write this blog.  Finally, I'll be using DosBox X for performance testing, with the CPU set to emulate the speed of a 25Mhz 286.

# Coding!
With that out of the way, I have made a good bit of progress with my project in my first day of coding.  First, I've accomplished sort of the equivalent of a "Hello, world!", a graphical screen that displays the title of my program.  I apologize for the weird colored patterns in the photos below.  That's just an artifact of my taking a picture of my monitor with my iPhone.
![Hello CARDIAC!](/IMG_1067.jpg)
This was no great achievemernt, of course.  In fact, it's mostly code just copied from one of the reference books I'm using, with some tweaking for my application.  But it allowed me to setup some of the basic architecture of my app and start refamiliarizing myself with C and Turbo C++ in particular.

My bigger accomplishments were to basically code the CARDIAC emulator and a set of unit tests to prove that it's working as intended.

This latter issue proved to be a little bit of an issue.  I first did this project in 1994 or 1995, before I was a professional programmer.  In fact, I'm completely self-taught (I was a history major in college), and at the time I wrote my first version of CARDIAC, I was just learning C/C++.  It was in fact my first project in that language.  So I don't know what concept of unit tests programmers had nearly 30 years ago, but I do know that Turbo C++ doesn't seem to have any support for unit tests.

![Unit tests](/IMG_1070.jpg)
I ended up rolling my own very simple unit testing approach, using a preprocessor definition to either build the main app or to build the unit tests.  It's not the most sophisticated unit testing framework by any means, and there are a ton of ways it could be improved, but for my needs with this relatively simple project it is good enough.
![Unit tests](/IMG_1069.jpg)
With the unit testing framework in place, I was able to complete the core coding on the CARDIAC emulator so that the memory, CPU, and all instructions work and pass unit tests.

# But what version of CARDIAC?
Thanks to eBay, I have acquired a copy of CARDIAC (something I actually didn't have the first time I did this project; I actually did it based on the description from a friend), but there are some issues that aren't covered by the manual.  This was meant as a teaching tool with processing done manually by students, so the specification didn't need to be as rigorous as an Intel specification manual.  This page on [CARDIAC](https://www.cs.drexel.edu/~bls96/museum/cardiac.html) describes the ambiguities and how they resolved them, and I've chosen to follow their example.  This page also has a lot of other detailed discussion on CARDIAC, so if you're interested in how it works or what you could do with this system, I highly recommend you check it out.