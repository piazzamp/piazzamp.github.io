---
layout: post
title: '2015 CSAW CTF: two forensics 100s flash & keep calm'
date: '2015-10-07T08:01:00-05:00'
tags:
- csaw
- ctf
- hacking
tumblr_url: http://mattpiazza.com/post/130678536047/2015-csaw-ctf-two-forensics-100s-flash-keep
---
these two forensics challenges were especially easy – perhaps I just found skiddie workarounds or something. but, points-wise, they were on the same level as notesy so their ease makes some sense :)

keep calm and ctf

the challenge presented us with an image and told us it was special. steganography was likely a force at play.


above is the way I actually solved this one (with david). highlighted, you can see the flag. this solution was inspired by the photo of yoshi in fuzyll’s recon challenge. both of these images contained stegonagraphic data inside of an image. for yoshi, I tried to view the source to look for html comments or some other easy hint like that, but instead I saw this steganographic stuff. nice nice.
an easier (and more 1337 way to find this) is to combine xxd and vim into a mythical hexeditor. thanks to my coworker, king, for showing me this recently.
basically, open img.jpg with vim and type :%!xxd which, if I am interpreting it correctly, runs xxd on the file (aka %) and writes the results back into the buffer. see below.
 
then just slam :q! to exit without saving the hexdump. or you could, ya''know, just hexdump outside of vim.

flash

the challenge statment for this one was: We were able to grab an image of a harddrive. Find out what''s on it. tough. I downloaded the file and ran strings on it to see if there was anything especially juicy. the last printable string in the image turned out to be the flag:
 
that’s it. welcome to ctfs. flags just sort of plop out of files like that sometimes.
