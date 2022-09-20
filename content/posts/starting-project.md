---
title: "What I want to do"
date: 2022-09-19T16:39:22-04:00
draft: false
---
I will use Borland's Turbo C++ 3.0 to write a graphical CARDIAC emulator for MS-DOS.  My project will have several phases:
1. Setup my development environment.  I will use an emulator for MS-DOS, and will install Turbo C++ and whatever other software I might need.  Since I'm on an M1 Mac, most of the virtual machine solutions like VMWare and VirtualBox won't be available to me.  But I should be able to use QEMU to emulate an x86 environment.
2. Develop the CARDIAC program using the Borland Graphics Interface (BGI) library that is included with Turbo C++.
3. I don't anticipate taking more than a few days or a week to finish the CARDIAC emulator, so the second phase of my project will be to replace the BGI library with my own graphics library.  
4. Lastly I will setup a computer running MS-DOS to test my program on real hardware.

# Why do I want to do this?
This is a project I first did about 30 years ago, and it still stands out in my mind as one of my favorites.  At the time, I was in college and working nights in an office where I had a lot of free time and access to a computer.  So I purchased Turbo C++ via mail order and taught myself C and C++, which are still among my favorite languages.

My goals for this project are:
1. To see how differently I would tackle this project after decades of professional coding experience.
2. To see how the programming experience differs using the first IDE that I ever worked with compared to what I'm used to today.
3. To relive one of my favorite programming projects and drink in the nostalgia! :smiley:

# What is CARDIAC?
![My copy of CARDIAC](/IMG_1056.jpg)

CARDIAC is a teaching tool produced by Bell Labs in 1968.  Most people didn't access to a computer to learn on, so the authors developed a cardboard computer utilizing the student's brain as the CPU.  

Architecturally, it was a very simple computer.  It had only 100 memory addresses, referenced as 0 to 99.  Each memory location could store 3 digits and a sign (the computer used decimal numbers not binary), with the range of values being from -999 to +999.  It had only two registers, the Accumulator and the Program Counter, and had only 10 instructions.

When I wrote my program many years ago, I kept with the training tool theme and gave the user the ability to step through code, visually indicating which instruction was being updated and which memory address was being read or written to.  The user could see a dissambled version of their code, and they could even enter code as a mneumonic or just the numeric equivalent.  I even wrote a command-line assembler for it.

[CARDIAC at Wikipedia](https://en.wikipedia.org/wiki/CARDboard_Illustrative_Aid_to_Computation)

