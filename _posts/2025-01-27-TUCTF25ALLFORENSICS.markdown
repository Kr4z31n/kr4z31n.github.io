---
layout: post
title: "TUCTF 25 Writeups"
date: 2025-01-27 15:21:27 +0530
categories: ctf
description: All forensics writeups for TUCTF25
media_subpath: /assets/postimg/TUCTF25/
path: TUCTF.png
alt: "TUCTF - 2025"
image: TUCTF.png
tags: [TUCTF25,Network,Memory,Artefacts]
---




## Mystery Present [Forensics]

**Difficulty:** Easy
**Solves:** 318

#### Description 

```
We recently got this absolutely non-sensical presentation from a confidential informant, along with a notes that said "The truth hurts boomers,
 but it's what on the inside that counts <3". We can't make heads or tails of it, but it has to be important! Can you help us out?
```

#### Flag `TUCTF{p01yg10+_fi1e5_hiddin9_in_p1@in_5i9h+}`

Given was a `.pptx` file as handout. Running foremost on it or renaming the .pptx file to .zip we get a `secret_data.7z` file which has the flag.txt

```
TUCTF{p01yg10+_fi1e5_hiddin9_in_p1@in_5i9h+} 

Polyglot files are pretty cool, here are some way more advanced ones: https://github.com/angea/pocorgtfo
```

## Packet Detective [Forensics]

**Difficulty:** Easy
**Solves:** 350

#### Description

```
You are security analyst given a pcap file containing network traffic. Hidden among these packets is a secret flag transmitted. 
Your task is to analyze the pcap file, filter out common traffic, and pinpoint the packet carrying the hidden flag.
```

#### Flag `TUCTF{N3tw0rk_M4st3r}`

Given was a `.pcap` file. Goin through the packet paylaods one by one we could see the flag in plainsight.


## Security Rocks [Forensics]

**Difficulty:** Medium  
**Solves:** 143

#### Description 

```
I shared a super secret message, I hope its secure.
```
#### Flag `TUCTF{w1f1_15_d3f1n173ly_53cure3}`

Given was a `.cap` file. Analysing the packets I headed straight for `aircrack-ng` [download here](https://aircrack-ng.org/).

Using the command 

```Powershell
aircrack-ng.exe .\dump-05.cap -w .\rockyou.txt
```
I was able to crack the WPA2 password and used the essid and passphrase to decrypt the cap file.

```Powershell
airdecap-ng.exe -e securityRocks -p youwontguessit92 dump-05.cap
```

On analysing the the decrypted logs we could find there was a file named
`secret.txt` passed across the network. Extracted the file which had the flag which was `base62` encoded.

```
Heres my super secret flag, I made it extra secure ;)
1KZTi2ZV7tO6yNxslvQbjRGL54BsPVyskwv4QaR29UMKj
```

A pretty good guide for password cracking [here.](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://sites.wp.odu.edu/ashields/wp-content/uploads/sites/36136/2024/04/AShields-CYSE-301-Assignment-5-Password-and-WiFi-Cracking-11-19-23-compressed.pdf)

## Bunker [Forensics]

**Difficulty:** Medium  
**Solves:** 66

#### Description 

```
[REPORT] Mail notification received from access point. Subject: YoRHa_CLASSIFIED_12u3440029491.

Origin: Bunker.

Query: What is the nature of this transmission?

Hypothesis: Perhaps a deeper analysis of the files enclosed can yield more information.

Proposal: Scanner-type unit is advised to perform standard scanning procedures on the data for further analysis and conclusion.

Warning: Data appears to be classified. Any acts related to hacking and unauthorized transactions could be associated with treason. Unit is advised to perform such acts in secrecy.
```
#### Flag : `TUCTF{Th1s_C4nn0T_ConT1nu3}`


Given as handout was `Bunker_DB` and `Bunker_DMP` which were a kdbx database and dmp file.

Using this tool [https://github.com/vdohney/keepass-password-dumper] 
https://github.com/vdohney/keepass-password-dumper
on the DMP file i was able to retrieve the kdbx master password.
Running the tool would give an output like
```bash
Password candidates (character positions):
Unknown characters are displayed as "●"
1.:     ●
2.:     L, Ï, , §, y, H, , q, $, W, A,
3.:     0,
4.:     R,
5.:     y,
6.:     _,
7.:     2,
8.:     _,
9.:     M,
10.:    4,
11.:    n,
12.:    k,
13.:    1,
14.:    N,
15.:    d,
16.:    !,
17.:    _,
18.:    Y,
19.:    0,
20.:    R,
21.:    H,
22.:    4,
Combined: ●{L, Ï, , §, y, H, , q, $, W, A}0Ry_2_M4nk1Nd!_Y0RH4
```
The master password would be the combined string of characters from the output.

Giving us the master password as :
> gL0Ry_2_M4nk1Nd!_Y0RH4

Looking into the entries of the kdbx file we go through the history of the entry in the Recycle Bin to get the flag from the password.
