---
excerpt: "Robot was attached to parallel port for communication. It moved front and
  back and rotated, without motion in x or y direction, in 360 degrees, clockwise
  as well as anti-clockwise.\r\n\r\nA camera was mounted on top of the robot. I used
  mobile phone so that the video captured can easily be streamed from the phone to
  the computer.\r\n\r"
categories: [page, project]
layout: page
title: Network controlled Robot with vision
created: 1268975954
---
Robot was attached to parallel port for communication. It moved front and back and rotated, without motion in x or y direction, in 360 degrees, clockwise as well as anti-clockwise.

A camera was mounted on top of the robot. I used mobile phone so that the video captured can easily be streamed from the phone to the computer.

Java was used as both front-end and back-end. The program could communicate with robot using parallel port. The 2 motions and 2 rotations were encoded and connections were made appropriately. MAX232 received these signals and provided the 12V DC adapter powered DC motors with appropriate voltage.

The streamed video was further streamed from the server system across the LAN. The client in the LAN was used to control the robot, the commands send by which were received by the server to which the robot was connected. Also the client, receiving the video from the server, displayed it to the user. Java provides good APIs (Java Media)for this purpose.

(Java)
