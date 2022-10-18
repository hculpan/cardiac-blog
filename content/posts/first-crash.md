---
title: "First Crash"
date: 2022-10-06T20:42:07-04:00
draft: false
---

I've been busy for the past few days, so today was the first time I got to do some coding after starting the project. I made some good progress, essentially building the entire visual display except for a few small details. There's no user interactity yet, but at least the user can see the state of the CARDIAC emulator.
![CARDIAC interface](/IMG_1072.png)
Note the image above is CARDIAC running in DosBox on my Mac.

Overall, I'm pleased with the interface, and it mostly matches what I did 30 years ago, at least as much as I can remember. In addition to getting the visual interface mostly complete, CARDIAC now executes a program on launch. Unfortunately, right now there isn't much for it to do. The system starts executing code at address 00, which is ROM with the instruction 001. This instruction gets user input and put the number the user inputs at address 01. But I haven't written the input routine yet, so right now I just hard-coded it to always assume the user input is 0. So it puts 000 in address 01, and then moves forward to 01 and attempts to execute.

The instruction 000 at 01 will then try to get user input and store it in address 00. Since 00 is ROM, this can't work, and the manual didn't address what happens when you try to write to ROM. My CARDIAC emulator won't allow this to happen, and originally it would just do nothing and move on to the next instruction. But I changed this to cause the emulator to halt. So this is what you see above. The emulator has executed 001 at address 00, then moved to address 01 to execute 000, which then halts the emulator. Thus the PC is set to 01 and not 00. I'm not sure if I'll keep this behavior of halting on instruction 000 or not, but for right now it's useful for my testing.

In the process of writing this code, I did run into my first run-time crash.
![CARDIAC having a cardiac event](/IMG_1071.jpg)
I'm using sprintf() to format my numeric data to strings so I can draw them on the screen. But for the memory display, I forgot to put a null terminator on my generated string. So the text draw routine would draw text not just from the character array but from whatever random bits were in memory right after this. So it locked up the computer every time. It wasn't a difficult issue to figure out, though it took considerably longer because I kept having to wait for my computer to reboot.

I had one other issue that took more time to figure out. As it happened, though, it was a stupid mistake on my part. I created a boolean type since, unlike more modern C implmentations, there is no such standard type. Of course, the usual method is just simply to #define 0 FALSE/#define 1 TRUE. But I decided to be a little fancy and create an enumerated type with true/false values. The problem with this is that you can't just check if true in the more abbreviated for of "if (a) ...". Instead, you have to "if (a == true)...". In this particular case, I just forgot to do that, and a loop that I was expecting to run for a while ran only once and immediately broke out. Honestly, it was stupid to use an enum-based boolean type, so I'll probably go back and change it.

While it was a stupid bug, it did give me an opportunity to try out Turbo C++'s debugger. As an integrated development environment (IDE), the debugger is built into the Turbo C++ system, so there's no need to drop into another program like GDB or something similar. I've used many debuggers over the course of my career, and while I wouldn't rank Turbo C++'s as my top choice, it is still not bad at all. It does most of what you'd want from a debugger, and does so with as easy an interface as you could expect. Given that it's 30 years old, I think it compares favorably with modern debuggers. The biggest limitation is really not it's fault. It's text-based, which means the interface just can't be as intuitive as a modern GUI debugger. But I think the Borland developers did a good job with what they had, and it's definitely a very usable tool.

# Performance

As you may have noted from the screenshot above, I did my first performance test today by running CARDIAC on DosBox on my Macbook Pro. I'm technically using DoxBox X, where it's easy to change the performance based on CPU you want to emulate. For this project, I've chosen a 25Mhz 286 CPU as my minimal configuration that I'm supporting, and I'm pleased to say that it works just fine. It's not instantaneous; it takes about 2 seconds to draw the whole screen. But updates should be far faster, so I think this slight delay on startup is very liveable.

Just as an experiment, I did try running CARDIAC with the CPU set at 4.77 Mhz 8088 speed. Obviously it ran much slower. It took roughly 12 seconds to draw the entire screen. However, once the screen is loaded, I should be able to do much faster updates, so I think it will be perfectly usable there as well. This isn't an action game, so instant frame updates aren't a necessity, so the currently performance seems like it will be good enough.
