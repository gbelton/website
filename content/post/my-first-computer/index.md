---
title: My First Computer
author: ''
date: '2017-01-02'
slug: my-first-computer
categories: [Hardware]
tags: []
image: 
  caption: ''
  focal_point: ''
---
 
Photo courtesy of Dan Veeneman

Sometime in the late 1970’s I bought my first computer, an ELF II from Netronics. It arrived at the house in a big, padded envelope. Inside the envelope was an empty printed circuit board, and several little plastic bags containing the electronics. I spent a Saturday afternoon with a soldering iron assembling and testing, and then proceeded to try to figure out what I would do with it.

This thing was certainly no supercomputer. It had a whopping 256 bytes of memory. Not megabytes, not kilobytes; just bytes. It featured an RCA COSMAC 1802 8-bit microprocessor. This chip’s main claim to fame was that it was designed to be highly resistant to radiation and electrostatic shock, which led to it being selected by NASA for a number of space projects. To a geeky teenager in the 70’s this gave it a lot of appeal, even though my little board was never going to be exposed to cosmic radiation.

The basic $99 kit was strictly a learning project. It included a two-digit hexadecimal display and a 16-key hexadecimal keypad, plus an Interrupt key and three toggle switches. Programming it consisted of using the toggle switches to step through memory, entering a hex code corresponding to a machine-language instruction, and then stepping to the next memory location. This was tedious, to say the least. Creating a program that did anything useful, or even interesting, in 256 bytes of memory was quite a challenge.

One of the most exciting features was the expansion bus. The board had room for five expansion sockets, and came with one socket plus a few SIP headers which could be used to connect to certain input/output signals. My first project with the ELF consisted of connecting a speaker to one of these sockets. By rapidly toggling the state of the output pin, I could produce tones from the speaker. By changing the speed of this toggling, I could produce tones of different frequencies. My crowning achievement used the interrupt feature of the chip to produce a tone only while a key was pressed, with a different tone for each key. That’s right, I created a primitive music synthesizer capable of producing sixteen different notes! Eat your heart out, Moog!

Just to give you an idea of what it was like programming one of these, here’s a code snippet:

```armasm
 0000 71        RESET   DIS                     ; DISABLE INTERRUPTS  
 0001 00                DC      0  
 0002 F8FF      FINDRAM LDI     0FFH            ; FIND RAM, STARTING AT FFFF  
 0004 B4                PHI     R4  
 0005 F8FF      TRYAGAIN LDI    0FFH            ; REPEAT...  
 0007 A4                PLO     R4              ; - TEST TOP BYTE ON PAGE  
 0008 54                STR     R4              ; - STORE 'FF'  
 0009 04                LDN     R4              ;   READ IT BACK,  
 000A FBFF              XRI     0FFH            ;   COMPARE  
 000C C6                LSNZ                    ; - IF OK, STORE ALL 0'S,  
 000D 54                STR     R4              ;   READ BACK,  
 000E 04                LDN     R4              ;   COMPARE  
 000F 321A              BZ      RAMFOUND        ; - IF OK, THEN RAM FOUND  
 0011 94                GHI     R4              ; - IF NO MORE PAGES TO TEST,  
 0012 32DD              BZ      NORAM           ;      THEN GO TO NORAM  
 0014 A4                PLO     R4              ;      ELSE DEC. PAGE NUMBER  
 0015 24                DEC     R4  
 0016 84                GLO     R4  
 0017 B4                PHI     R4              ; ...UNTIL DONE
```

Keep in mind that one doesn’t see all of this on your display. You start at memory location 0000, enter “71” on the keypad, flick a toggle switch to enter, flick another toggle switch to advance to memory location 0001, enter “00”, etc.

The expansion bus allowed much more, though. Netronics offered an expansion board that included an RS-232 serial port, a cassette interface for storing programs on tape, and a system monitor program that would display memory on a TV set or monitor. Other accessories included a prototyping board, a 4k RAM expansion, and a Tiny BASIC interpreter. Users found a wide variety of creative ways to use these, as documented in the Netronics newsletter. In those pre-Internet days, the newsletter was the only way of keeping up with the activities in the ELF community. Those prototyping features made the ELF II a predecessor of products like the Arduino and the Raspberry Pi.

When I went looking for information online to illustrate this post, I was surprised to find that the COSMAC lives on, in the form of an active retro computing community. If you want to learn more, a good starting point is http://www.cosmacelf.com/.  If you want to get in on the fun, a modern version of the original ELF computer is available, designed to fit in an Altoids tin.

Dan Veeneman also has a lot of information at his Decode Systems website. I’d like to thank Dan for giving me permission to use the photo at the top of this post.