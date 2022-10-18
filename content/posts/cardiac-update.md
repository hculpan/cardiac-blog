---
title: "Mid-month check-in (part 1)"
date: 2022-10-13T12:39:56-04:00
draft: false
---

Time for my mid-month check-in, and though I haven't posted in a week, it doesn't mean I haven't been busy. Because I've accomplished two big goals, I'll actually be breaking this entry into two entries. So expect the "part 2" in a day or two. If you're uninterested in ready this blog, I have a link to a video below that will cover most of what I say below.

![CARDIAC interface](/IMG_1073.png)

If you've been following this project, the screenshot above should look pretty familiar. The only differences are the bootloader instructions on the left hand side, and the status indicator in the lower left hand corner. But while it's not obvious from the images, there's a lot more under the hood. Specifically the OUT works, as you can see from the image. the INP also works, though this screenshot doesn't show that. But I have a video (link below) that show off all the functionality.

The status indicator in the bottom-left would normally show "RUNNING" while CARDIAC is executing a program. It only goes to "HALTED" if it encounters an HRS instruction or if an instruction tries to write to address 00, which is ROM. I've added the ability to restart CARDIAC by pressing F5 or F7. The difference is that F5 starts execution at where ever the PC points to, while F7 resets the PC to 00 and then starts execution there.

The last enhancement I made to CARDIAC was to be able to ready data into memory on launch, rather than requiring one to type it in manually everytime. Honestly, this was just a quality-of-life kind of enhancement, because during testing I found myself entering the same program over and over again. The format for the "executable" file is very simple. It's just a text file, and each line has the address and value to put in that address separated by a colon. It doesn't have to be in order, and you could even have the same address multiple times, but of course this would be a "last write wins" situation. The "addnums.cbf" is a simple program to just ask the user for two numbers and to add them. "CBF", by the way, stands for CARDIAC binary file. It's not a binary file, of course, since it's a text file, but for the CARDIAC platform it serve the same purpose as a binary executable file would on other platforms.

![CARDIAC machine language](/IMG_1075.png)

# Assembler

Entering in a program by hand is tedious. You basically have to write the assembler code, then convert it to machine language (in this case, 3-digit instructions rather than binary), and then finally enter it in manually. So of course I created an assembler.

![CARDIAC machine language](/IMG_1074.png)

If you're curious about the specifics of the above assembly language program, please watch the video below where I discuss it line-by-line. But to summarize, this program asks the user for two numbers, adds them, and outputs the result. To assemble into program form, you call the same CARDIACE.EXE, passing in the "ASSEMBLE" command and the filename without the extension (it assumes ".CAF"). It is a pretty basic assembler, but for a 10-instruction 100-memory computer, you don't exactly need MASM! There are still some enhancements I'd like to add, but right now it's probably about 80-90% complete.

# Future enhancements

For CARDIAC, there's not a whole lot more I want to do before I can call it "feature coplete". There is a built-in delay during execution that is currently hard-coded, so I want to add a way for the user to adjust this. The second enhancement is with the delay itself. Right now I use the C function, "delay()", to do this. The problem is, the computer basically locks up during this time, which means that if you, for example, hit ESC to terminate, it may take a second or so before this happens. So I want to change this to a simple loop that just checks the amount of time that has passed, so it can check the keyboard immediately for any user action. Both of those are very small enhancements, of course, which is an indication of how close to feature complete CARDIAC is. That means I'll be able to move onto "Phase 2" of this project very soon.

For the assembler, I have a bit more work to do, although still pretty small changes. At the moment, when you use the DATA instruction, you have to provide the memory address where this will be stored. I want to give the user the option to let the assembler determine the address.

The other more important feature that is missing, though, is code labels. Right now, if you use the JMP instruction, you'll have to specify the instruction. Ideally, you should be able to add a label to a line of code, and then use that label in a JMP, TAC, or HRS instruction.

# CARDIAC the Movie

This covers pretty much everything I talked about above, with the exception of the "Future enhancements".

[Mid-month CARDIAC video](https://youtu.be/BHdIhzmKadE)
