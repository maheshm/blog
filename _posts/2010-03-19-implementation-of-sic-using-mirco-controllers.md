---
categories: [page, project]
layout: page
title: Implementation of SIC using mirco-controllers
created: 1268975832
---
This was my mini-project. Many didnt even understand what the title meant. If you are one among them here it is explained: SIC is a theoretical or hypothetical computer. It is used to explain the basics of how a computer functions. My aim was to implement this computer.

This could be used as a study-kit. This was implemented using 2 micro-controllers - one acted as the processor and the other as the RAM. MC1(processor) communicated with MC2(RAM) using its ports. The communication path was multiplexed and it was used to transmit both data and instructions. Instructions were coded just as it was done in SIC. It even had some instructions from SIC/XE.

In between MC1 and MC2, ie in the data/instruction path, LEDs were kept so that the user can actually see the request and reply cycles between the two. In other words on can see the request from MC1 followed by MC2's reply.

Python was used to translate(assemble) the assembly code into binary codes and upload it to MC2(RAM). Then a trigger starts the MC1(Processor) to start fetching from address 00h.

I am trying to get my code as the system in which the whole project was done had a hard disk failure.

(C, Embedded System)
