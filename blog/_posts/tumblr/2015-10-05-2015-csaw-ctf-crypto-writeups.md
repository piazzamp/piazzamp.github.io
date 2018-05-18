---
layout: post
title: '2015 CSAW CTF: crypto writeups'
date: '2015-10-05T08:01:18-05:00'
tags:
- csaw
- ctf
- hacking
tumblr_url: http://mattpiazza.com/post/130544587001/2015-csaw-ctf-crypto-writeups
---
all of the 50-point crypto challenges this year were based on Mr Robot episodes – the ciphertext files were named after episodes while all the flags were quotes from that episode. neat.

eps1.1_ones-and-zer0es_c4368e65e1883044f3917485ec928173.mpeg

we were given the following ciphertext:
011001100110110001100001011101000111101101010000011001010110111101110 000011011000110010100100000011000010110110001110111011000010111100101 110011001000000110110101100001011010110110010100100000011101000110100 001100101001000000110001001100101011100110111010000100000011001010111 100001110000011011000110111101101001011101000111001100101110011111010 010000001001001001001110111011001100101001000000110111001100101011101 100110010101110010001000000110011001101111011101010110111001100100001 000000110100101110100001000000110100001100001011100100110010000100000 011101000110111100100000011010000110000101100011011010110010000001101 101011011110111001101110100001000000111000001100101011011110111000001 101100011001010010111000100000010010010110011000100000011110010110111 101110101001000000110110001101001011100110111010001100101011011100010 000001110100011011110010000001110100011010000110010101101101001011000 010000001110111011000010111010001100011011010000010000001110100011010 000110010101101101001011000010000001110100011010000110010101101001011 100100010000001110110011101010110110001101110011001010111001001100001 011000100110100101101100011010010111010001101001011001010111001100100 000011000010111001001100101001000000110110001101001011010110110010100 100000011000010010000001101110011001010110111101101110001000000111001 101101001011001110110111000100000011100110110001101110010011001010111 011101100101011001000010000001101001011011100111010001101111001000000 111010001101000011001010110100101110010001000000110100001100101011000 01011001000111001100101110

obviously this is supposed to represent some binary data. I googled ’binary to ascii’ and pasted in the string. which is kind of funny since it’s ascii to ascii, really (or, like, ascii to unicode or something). episode 1.1, easy as pie.

eps1.7_wh1ter0se_2b007cf0ba9881d954e85eb475d0d5e4.m4v

this time we were given ciphertext that looked to be produced by a standard rotation or substitution cipher
EOY XF, AY VMU M UKFNY TOY YF UFWHYKAXZ EAZZHN. UFWHYKAXZ ZNMXPHN. UFWHYKAXZ EHMOYACOI. VH''JH EHHX CFTOUHP FX VKMY''U AX CNFXY FC OU. EOY VH KMJHX''Y EHHX IFFQAXZ MY VKMY''U MEFJH OU.
me, elaina, sarah, and callum swarmed to the file for some quick frequency analysis. such a short piece of ciphertext could only get us so far. sarah had taken some sort of cryptology course in estonia so she told us to examine the frequencies of n-grams. that also gave us some strong candidate translations. but what ended up being the key, was to simply look at the ciphertext and figure out how it could fit into the english language. we focused on EOY and TOY – EOY has to be a word that can start a sentence or phrase and its last two letters must make sense as the last two letters of another 3-letter word. the same tactic was applied to XF (word 2) and YF (word 8). we attempted to uncover the substitution alphabet one or two characters at a time, then translate the ciphertext with each character in the candidate substitution alphabet lowercased so that we would be able to more easily see which character we still had to uncover. the apostrophes helped a lot – VH''JH was likely we''re or we''ve and each of the single characters following an apostrophe was likely a t, s, or maybe even a d. after applying some of these assumptions, more patterns became apparent and some partially decrypted words had an obvious plaintext value that lead to the discovery of more of the substitution alphabet. here’s a glamour shot of the finale to this solve:



eps1.9_zer0-day_b7604a922c8feef666a957933751a074.avi

here we started with some base64 encoded data (could it be that easy?!):
RXZpbCBDb3JwLCB3ZSBoYXZlIGRlbGl2ZXJlZCBvbiBvdXIgcHJvbWlzZSBhcyBleHBlY 3RlZC4g\nVGhlIHBlb3BsZSBvZiB0aGUgd29ybGQgd2hvIGhhdmUgYmVlbiBlbnNsYXZl ZCBieSB5b3UgaGF2\nZSBiZWVuIGZyZWVkLiBZb3VyIGZpbmFuY2lhbCBkYXRhIGhhcyB iZWVuIGRlc3Ryb3llZC4gQW55\nIGF0dGVtcHRzIHRvIHNhbHZhZ2UgaXQgd2lsbCBiZS B1dHRlcmx5IGZ1dGlsZS4gRmFjZSBpdDog\neW91IGhhdmUgYmVlbiBvd25lZC4gV2UgY XQgZnNvY2lldHkgd2lsbCBzbWlsZSBhcyB3ZSB3YXRj\naCB5b3UgYW5kIHlvdXIgZGFy ayBzb3VscyBkaWUuIFRoYXQgbWVhbnMgYW55IG1vbmV5IHlvdSBv\nd2UgdGhlc2UgcGl ncyBoYXMgYmVlbiBmb3JnaXZlbiBieSB1cywgeW91ciBmcmllbmRzIGF0IGZz\nb2NpZX R5LiBUaGUgbWFya2V0J3Mgb3BlbmluZyBiZWxsIHRoaXMgbW9ybmluZyB3aWxsIGJlIHR o\nZSBmaW5hbCBkZWF0aCBrbmVsbCBvZiBFdmlsIENvcnAuIFdlIGhvcGUgYXMgYSBuZX cgc29jaWV0\neSByaXNlcyBmcm9tIHRoZSBhc2hlcyB0aGF0IHlvdSB3aWxsIGZvcmdlI GEgYmV0dGVyIHdvcmxk\nLiBBIHdvcmxkIHRoYXQgdmFsdWVzIHRoZSBmcmVlIHBlb3Bs ZSwgYSB3b3JsZCB3aGVyZSBncmVl\nZCBpcyBub3QgZW5jb3VyYWdlZCwgYSB3b3JsZCB 0aGF0IGJlbG9uZ3MgdG8gdXMgYWdhaW4sIGEg\nd29ybGQgY2hhbmdlZCBmb3JldmVyLi BBbmQgd2hpbGUgeW91IGRvIHRoYXQsIHJlbWVtYmVyIHRv\nIHJlcGVhdCB0aGVzZSB3b 3JkczogImZsYWd7V2UgYXJlIGZzb2NpZXR5LCB3ZSBhcmUgZmluYWxs\neSBmcmVlLCB3 ZSBhcmUgZmluYWxseSBhd2FrZSF9Ig==

this one was solved by ethan.
if you aren’t familiar with base64 encoding, I googled it for you. but essentially, it’s a way to encode binary data to printable ascii characters. it’s used in http basic authentication. you can put binary data in-line in an html file with this sort of encoding.
immeiately I try base64 -D eps1.9_zer0-day_b7604a922c8feef666a957933751a074.avi only to see Invalid character in input stream. I determined that it was the backslash (\) that was invalid. so I removed them all and everything came out screwey when I went to decode it again. since each character represents 5 bits, incorrectly removing a single character from base64 encoded data will misalign the following bits. anyway, ethan later told us that the \ I was removing should have been \n – the standard newline escape sequence. thus: sed ''s/\\n//g'' eps1.9_zer0-day_b7604a922c8feef666a957933751a074.avi | base64 -D -. a few things are going on in this command. first the sed command contains two backslashes \\ to escape a single backslash so that the pattern matches the \n in the input string. then there are two forward slashes // which is normally where we would put the string to replace \n with, but we’re just replacing it with nothing. then the - at the very end, this is to represent stdin ie base64 will decode whatever comes into stdin.

very cool. 150 points in the bag.

notesy

next is a 100-point challenge that unfortunately eschews the Mr Robot theme. the challenge is down now so I missed my opportunity to take a gnarly screen grab (jk it was just a input textbox on a page). anything you put in this textbox would be ‘encrypted’ by what seemd to be yet another substitution cipher. competitors in the #isislab irc channel were flipping out about this one. I admit to having similar frustrations as them – there seemed to be little direction to the challenge. it was easy to figure out what was going on but not to figure out what the flag should be. later on the CSAW team put up some good hints.


these hints came, word for word, from the irc channel. anyway they lead me to the solution – the substitution alphabet which can simply be recovered by entering in the normal alphabet into the encrytor: abcdefghijklmnopqrstuvwxyz if you’re not familiar. once you know how the characters are rearranged you have broken the cryptosystem and can go back-n-forth between plaintext and ciphertext as you please.
ps – the youtube link in hint no. 2 goes to nic cage teaches the alphabet since you just had to enter the abc’s to solve the challenge.
