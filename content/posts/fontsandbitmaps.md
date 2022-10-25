---
title: "Fonts And Bitmaps"
date: 2022-10-22T12:49:59-04:00
draft: false
---

I'm pretty happy with the progress I've made with Splatter! so far, but it lacks a number of important features still. The biggest is probably its inability to either load or save images. This is an issue not just because the user can't save their beautiful artistry, but also because most programs use images to draw interface elements. For an environment like DOS where there's no built-in UI library like you have with Windows or Mac, you use images for almost everything that's not a basic box.

![Splatter! basic interface](/IMG_1080.png)
For Splatter!, the current interface has two major components, the drawing canvas and the color pallette. These are both drawn with graphic primitives, because lines and boxes are both easy and fast to draw this way. But more complex, less-boxy graphics are usually fastest done using bitmap images rather than graphic primitives. Examples of such elements would be buttons, scrollbars, and even text characters. Now sharp-eyed readers might notice that I'm already drawing letters, specifically the title, "Splatter!".

# Text and Fonts

I have two primary sources for learning about VGA programming, the first being [David Brackeen's 256-Color VGA Programming in C](http://www.brackeen.com/vga/index.html). But he doesn't cover text output. The other is Andre LaMothe's excellent book, [Teach Yourself Game Programming in 21 Days](https://www.amazon.com/Teach-Yourself-Game-Programming-Cd-Rom/dp/0672305623/ref=sr_1_4?crid=1U3U6BS91IP89&keywords=teach+yourself+game+programming+in+21+days&qid=1666459457&sprefix=teach+yourself+game+programming+in+21+days%2Caps%2C106&sr=8-4). Obviously, this is an old book as it focuses on exactly what I want to do, write a game in DOS using VGA's Mode 13h. So if you want to learn how to write a modern game, this probably isn't the book for you.

Brackeen's web site is very good, and it has code examples for DJGPP, which is the compiler I'm using. But it is a relatively small site, so he's generally more concise and doesn't cover the breadth of topics that LaMothe's book covers. For the case of text output, Brackeen's site doesn't touch on the topic at all, while LaMothe's book has a nice section on using the PC platform's built-in font to draw text.

As LaMothe explains, when you're running in text mode, the programmer tells the computer to display an 'A', for example, but the programmer doesn't tell the computer how to display this character. And the VGA card doesn't know how to do this either. Instead, the PC ROM has a bitmap of the entire ASCII character set that's used for text mode. It's an 8x8 bitmap, with each bit mapping to a pixel on screen. When I say "bitmpa", it is exactly that, a map of bits. So each row of a character is 8 bits or 1 byte, so each character only takes 8 bytes to define. This is stored as an array in ROM, so it's easy to access using the ASCII value as the index into this array.

An 'A', for example would look like:

```
byte 0: 0 0 0 1 1 0 0 0
byte 1: 0 0 1 0 0 1 0 0
byte 2: 0 0 1 0 0 1 0 0
byte 3: 0 0 1 1 1 1 0 0
byte 4: 0 0 1 0 0 1 0 0
byte 5: 0 0 1 0 0 1 0 0
byte 6: 0 0 0 0 0 0 0 0
byte 7: 0 0 0 0 0 0 0 0
```

Of course, you can't just say display an 'A' in graphics mode, but what you can do, and LaMothe has sample code to do this, is read the bitmap data for the character from ROM, and then simply translate that to the graphics screen. You just have to step through each bit to check if it's a 1 or 0, and then display the corresponding pixel on the screen appropriately.

So using this technique, I was easily able to write "Splatter!" on the graphics screen. The only decision I had to make was whether to support transparency for the pixels not being drawn by the font. In LaMothe's code, he had this as a flag passed in as a parameter. I decided I didn't want this minimal overhead, since I didn't forsee ever wanting it to be non-transparent. So I just removed the flag and the corresponding if-statement that tested it. If I ever want to draw characters without transparency, I figure I'll just create a second function to do so.

With that working, I still had a font problem. First, the built-in font in ROM is just not very pretty. The "Splatter!" title works, but it just doesn't look as good as it probably could. The second, and much more important issue, is that it's just big. You can't have every button and bit of text displayed at this size. LaMothe even addresses this briefly in his book, but rather than giving any information on how to do so, he just simply says to draw a new font in an image file, load the image file, and then display that on the screen.

The next logical step would have been to write the code to read image files, but this isn't what I did (I'll explain why in the section below). Instead, I defined the bitmap for my new font in code, using a hard-coded array. I needed it to be smaller than the ROM font. I initially tried to do it as a 4x3 font, but this wasn't enough to differentiate characters like 'N', 'M', and 'H'. They all ended up looking exactly the same. I finally settled on doing a 5x5 font, which was satisfactory for almost all characters.

To compare to the built-in font, my 'A' in this smaller font is:

```
  { // A
    { 0,1,1,1,0 },
    { 1,0,0,0,1 },
    { 1,1,1,1,1 },
    { 1,0,0,0,1 },
    { 1,0,0,0,1 }
  },
```

Generally, 5x5 was good for most characters. I could distinguish the 'N' from the 'M', and even more complex characters like the '&' came out okay. I made the decision not to support lower-case characters, as I didn't think 5x5 allowed me to do these. Another important different between the built-in font is that I didn't have blank rows as the ROM font does. So when drawing a string of characters, I had to add in a space between each character without assuming the font would do that for me. Similarly, the 8x8 ROM font had empty lower rows for characters like a ',' or lower-case 'y' where the bottom dips below the font baseline. I thought about increasing my font to 6x6 to support these features, but decided against it.

![Small font](/IMG_1081.png)

Here's a little test I did to display all the defined characters, with the original "Splatter!" title for comparison. Overall, I'm very happy with the result. The first thing to notice is that it doesn't display all characters, even considering that it won't ever display lower-case characters. The above characters are only the ones I've currently defined. Creating characters for my font is a little tedious, so I've tried to limit it to characters I think I'm likely going to want to display. For any other character, including non-printable characters that are defined in the ASCII set, my font routine just returns a space character, i.e., nothing. There are still some more I'll have to define before I can consider it complete, but it's probably 80-90% there already.

As to the quality, as I said, I'm very pleased, but there are a few that I'm unhappy with. The worst is the '$' (the 'S'-looking character) between the '#' and the '%' characters on the third line. I wrestled with this one for a while, but could never come up with anything better. All three of the games that I have in mind will use currency, so I definitely have to revisit this. Perhaps move to a 6x6 font? I don't know, but at least for the moment I don't need to worry about it. Not much need for a currency symbol in a paint program. A few of the others, like the '&' and the '?' I'm not completely satisfied with, but I think they do the job.

I'm also very happy with my approach of using the hard-coded array to define my font, rather than loading it from a file. First, it meant less code that I had to do right now, since it meant I didn't need to write the code to read in an image file. Second, my drawing skills are pretty rough, so defining a 5x5 font in code was more reliable for me than trying to draw it in a drawing program.

But it does have some downsides. First, each character is 5x5 bytes, not bits, so each character takes up 25 bytes. That 1.4k for the 59 characters I currently have defined. By the time I get all the characters defined that I need to, that will probably be closer to 1.7k. Even in real mode DOS that wouldn't be a big deal, but I'm in protected mode, so I have access to all 32M in my machine. So it's pretty trivial. Second, it requires me including - and thus compiling - another C module, which adds time to my already-slow compilation process. Of course, once I have the all the characters defined, it won't have to be rebuilt until I do a full build, so it's also not a big deal.

At some point I may create a new font to replace using the built-in font, but that's pretty low on my list of priorities.

# Image files

While building my font in code put off my havint to read in image files, this is something I will really have to do next. It's really not a big deal, it's just I'm debating with myself about which image format I'll support. It's down to BMP vs PCX. The former is the simpler of the two, with a pretty straightforward layout and no data compression. The only real downside is that it's more of a Windows format than a DOS one. For example, Deluxe Paint II, the only DOS paint program I have installed, doesn't support BMP but it does support PCX. But, of course, Paintbrush for Windows 3.1 supports BMP as well as PCX.

PCX, on the other hand, is a bit more complex. It uses Run-Length Encoding (RLE) compression. RLE is about the simplest compression algorithm that could be called a compression algorithm. Basically, when you have multiple pixels in a row that are the same color, you don't have to have a string of pixels with the same value. Instead, you just use 2 bytes to say, "the next 20 pixels use the 5th color in the pallette". This is 2 bytes vs 20 bytes. This obviously isn't overly complex to code, but it is more complexity than a non-compressed format - and it's added complexity that really doesn't get me anything, given my relative lack of memory or hard disk constraints. But it would be nice to be able to use Deluxe Paint II to create or edit images and then just pop back into my program, instead of having to start up Windows and use Paintbrush. I could, of course, just not compress my images, but I'd still have to support the RLE for reading since Deluxe Paint II presumably will write out images using it.

Neither is particularly difficult, and I may just end up supporting both. LaMothe's book covers PCX, and Brackeen's site covers BMP, each with code for these formats. And of course there are plenty of other online and book resources for either of these formats. So with neither am I having to reinvent the wheel, as it were. So either or both should be relatively simple to do.
