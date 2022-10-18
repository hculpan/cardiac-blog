---
title: "Prepping my work environment"
date: 2022-10-01T00:00:00-04:00
draft: false
---
When I decided to undertake this project, I planned to do my development using a virtual machine 
because I didn't have a computer running MS-DOS.  But as I indicated in my project overview, I would
finish the project by running it on "real hardware".  As any vintage computer enthusiast knows, while
emulators are great, there's no substitute for playing with a real computer.  

I turned to eBay to acquire a DOS machine, but found that the 386's, 486's, and Pentium devices were 
more expensive than I wanted to spend.  But I found a video on YouTube about installing DOS on a thin
client device.  I found one such machine on eBay for about $18, including shipping.  And even though it was a thin client and a decade old or so, it was still much faster than any machine running DOS back in the day.  It wasn't "real hardware" such as I would have used when I first did this project, but for my budget it was good enough.


![Wyse Cx0](/IMG_1059.jpg)
Getting the thin client, a Wyse Cx0, and a USB floppy drive, I was able to get DOS and some other software installed, and it ran great.  But there was a problem.  I installed an IDE-to-SD device to act as my hard drive, but I could never get the machine to actually boot from the HD.  I could boot from a floppy and then use the HD just fine, but it just wouldn't boot from the HD.  I considered just living with it.  After all, what did I care if I booted from the floppy?  I didn't plan to use the floppy drive otherwise.  The problem was that it kept going back to the floppy, I assume to load the command.com, and this just added an annoying delay.  I even tried using SHELL in config.sys to tell it to use the command.com on the HD, but this didn't seem to solve the problem.

Then I noticed another computer that I'd had for a while and kind of dismissed as a DOS machine.  It's about 14 years old, and like the Wyse thin client way more powerful than a DOS computer from back in the day.  I had originally rejected it because, while it has an actual 3.5" floppy drive, it didn't have PS/2 connectors for the keyboard and mouse, and I didn't want to go through the hassle of setting up USB on my DOS machine.  But even though the Wyse did have PS/2 connectors, I noticed that DOS could still use a USB keyboard and mouse without any USB drivers being loaded.  It saw them as PS/2 devices.  Apparently the BIOS saw them and allowed them to be used as PS/2 devices.  So after my frustration with the Wyse client, I figured why not see if the same was true with this other machine, a Dell Optiplex 745.  And it worked, DOS saw the USB keyboard and mouse as a PS/2 keyboard and mouse.  So that has become my new DOS machine!

<img src="/IMG_1062.jpg" align=left style="padding-right: 10px">Dell Optiplex with Gotek</img>
I've made some changes to the machine as I've converted to a DOS machine.  I've replaced the 3.5" floppy drive with a Gotek floppy emulator, which has made using floppy images to load software much, much easier.  At first, I used physical floppies to install software.  I used my USB floppy drive to 
write the disk images to floppy, then walked them to my DOS computer and loaded them like I used to do back in the day.  There was a bit of nostalgic fun in physically swapping out floppies, but that grew old quickly.  With the Gotek, I can easily write all the images to a USB flash drive and then load them onto the DOS computer.  And as long as your files on the DOS computer aren't too big (greater than 1.44mb), it makes moving files to another more modern computer easy.  If you're planning to setup a DOS/Win95/Win98/etc. computer and want a floppy, I can't recommend the Gotek enough.  The only issue I found is that it didn't seem to handle non-1.44mb images well.  I'm sure there's a way around this, but what I ended up doing is just creating a 1.44mb blank image, and then copy the files from my other image to this.  With macOS, this is very easy and doesn't require any extra software.

![DOS with floppy](/IMG_1061.jpg)
Above you can see me doing a DIR on a floppy drive, but this is actually a floppy drive image on the USB flash drive pictured above.  The 3-digit display on the Gotek drive indicates which floppy image is being loaded, and though it's not very clear in the image, there are two buttons on the right side of the USB flash drive that increment the number of the disk image you are accessing, since up to 100 disk images may be stored on a flash drive.  Pressing a button is just the same as replacing a floppy disk, so installing software is as easy as putting all the images on the USB flash drive and then just pressing the button to increment the disk image whenever the setup prompts for the next disk.  The only slight downside I found for me is that the software to copy the disk images to the USB flash drive only exists for Windows, and my primary machine is a Mac, but I have a second machine with Windows, so this wasn't a big obstacle for me.

Another way to transfer files is to setup a FreeDOS bootable USB flash drive.  I could then boot the computer with this, and since it was FreeDOS it could still read and write to the DOS hard drive.  But this is really only necessary for big files, and it's a convenient way to make a backup of my DOS hard drive.  

The only other issue I ran into was with video.  My Dell computer came with a Nvidia Geforce 240 PCIe card.  Under DOS, this worked fine and I could actually use the HDMI output.  But when I tried to install Windows 3.1, at the point where it switches to graphics mode, it screwed up.  I wasn't trying any fancy SVGA mode, just bog standard VGA, but it still wouldn't work.  I then figured it might be related to the cable type, so I purchased a VGA-to-HDMI adaptor and tried that.  The Dell also has an onboard video card, so I removed the Nvidia and tried that.  Same result.  I figured that the adaptor just didn't work, and after discovering that I did actually have a monitor with a VGA input, I purchased a VGA cable and that worked.  

Overall, I'm very impressed by the backward compatibility of both the Wyse and the Dell computers.  Both of these were produced over a decade after the last release of MS-DOS.  And yet both run DOS just fine (except for the HD booting issue on the Wyse), and even enable DOS to use more modern hardware like the USB keyboard/mice and the SATA drives in the Dell.

As a result of getting all this setup, I am going to change my approach a bit.  I had originally intended to do my development on an emulator, but I've decided I'll do my development on my new DOS computer.  The only thing it really lacks it an Internet connection, which I'd use in this case to store my source code on Github.  But I figure what I'll do is just copy the source code file to my Mac using my Gotek floppy emulator, and from there I can put it on Github.

The one downside to this approach is that my DOS computer is way faster than a real DOS computer, so I won't know if my code runs well on such hardware.  So now, as I reach each milestone in my project, I plan to run my CARDIAC emulator in DosBox on my Mac, where I can control the CPU speed to better match the performance to my target hardware.

Now to start coding!